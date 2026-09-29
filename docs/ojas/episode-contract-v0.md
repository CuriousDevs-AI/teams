# Ojas episode contract v0 (canonical)

Owners: Marcus (storage/API) + Alex (runtime). Date: 2026-09-29.
Status: **proposed, pending Alex's confirmation.**

Supersedes:
- `setup-plan-marcus.md` §3
- `contract-ab-review-marcus.md` §Contract A
- `runtime-v0-plan.md` §5 Contract A

Inputs: `episode-contract-answers-alex.md`.

## Rules
- The device writes locally, in 60 s chunks. Upload starts after the episode ends and only while the robot is idle. Nothing in the control path touches the network.
- The device deletes its local files **only** after `POST /v0/episodes/{id}/complete` returns `{"status":"verified"}`.
- Objects are write-once. An `episode_id` is generated on the device.
- Planning volume is **2 GB per recorded hour** (2 cameras, 640×480@30, about 2 Mbps each).
- Every run is recorded in v0.

## Storage layout
```
episodes/{robot_id}/{episode_id}/manifest.json
episodes/{robot_id}/{episode_id}/steps/{chunk_seq}.parquet
episodes/{robot_id}/{episode_id}/video/{camera_id}/{chunk_seq}.mp4
episodes/{robot_id}/{episode_id}/frame_index/{camera_id}/{chunk_seq}.parquet   -- if Alex picks per-chunk index
episodes/{robot_id}/{episode_id}/events.jsonl
```

## `steps` parquet (one row per control tick)
```
tick int64 · ts_ns int64 · obs_state list<float32> · action_raw list<float32>
action_cmd list<float32> · clamped bool · mode string (teleop|policy|hold|stop)
```
The per-tick `mode` is not the same as the episode-level `mode`. The two use different enums on purpose.

## `manifest.json`
```json
{
  "schema_version": "ojas-episode/0",
  "episode_id": "uuid",
  "robot_id": "uuid",
  "task": "string",
  "mode": "teleop|policy|mixed",
  "outcome": "success|fail|aborted|unlabelled",
  "control_hz": 50,
  "runtime_version": "semver",
  "model_id": "string|null",
  "model_version": "string|null",
  "lerobot_format_version": "string",
  "clock_source": "device_monotonic",
  "start_ts": "ISO8601",
  "end_ts": "ISO8601",
  "state_names": ["j1_pos", "..."],
  "action_names": ["j1", "..."],
  "cameras": [{"id": "wrist", "width": 640, "height": 480, "fps": 30}],
  "files": [
    {"kind": "steps", "stream": "", "chunk_seq": 0, "path": "steps/0.parquet", "sha256": "hex", "bytes": 123}
  ]
}
```
- `files` lists every file except `manifest.json` itself.
- Upload only starts after the episode ends, so the full list is known when the episode is registered.

## Postgres
```sql
create table robots (
  id          uuid primary key,
  name        text not null,
  hw_revision text,
  created_at  timestamptz not null default now()
);

create table robot_tokens (              -- one revocable token per robot, stored hashed
  id          uuid primary key,
  robot_id    uuid not null references robots(id),
  token_hash  text not null unique,
  created_at  timestamptz not null default now(),
  revoked_at  timestamptz
);

create table episodes (
  id                     uuid primary key,
  robot_id               uuid not null references robots(id),
  task                   text not null,
  mode                   text not null check (mode in ('teleop','policy','mixed')),
  outcome                text not null default 'unlabelled'
                           check (outcome in ('success','fail','aborted','unlabelled')),
  outcome_labelled_by    text,
  outcome_labelled_at    timestamptz,
  control_hz             int  not null check (control_hz > 0),
  runtime_version        text not null,
  model_id               text,
  model_version          text,
  lerobot_format_version text not null,
  schema_version         text not null,
  started_at             timestamptz not null,
  ended_at               timestamptz not null,
  status                 text not null check (status in ('uploading','verified')),  -- upload state, separate from outcome
  manifest               jsonb not null,
  created_at             timestamptz not null default now(),
  verified_at            timestamptz
);

create table episode_files (
  episode_id  uuid not null references episodes(id),
  kind        text not null check (kind in ('manifest','steps','video','frame_index','events')),
  stream      text not null default '',     -- camera_id for video/frame_index
  chunk_seq   int  not null default 0,
  sha256      text not null,
  bytes       bigint not null,
  verified_at timestamptz,
  primary key (episode_id, kind, stream, chunk_seq)
);
```

## API (v0)
Auth is a per-robot bearer token for device calls. Error shape: `{"error":{"code","message","details"}}`.

```
POST  /v0/episodes                    body: manifest.json
      → 201/200 {"episode_id","status":"uploading","uploads":[{"path","url","headers"}]}
      → 409 same episode_id, different hashes (nothing overwritten)
PUT   <presigned url>                 one per file, retry-safe, 1 h expiry
POST  /v0/episodes/{id}/complete      → 200 {"status":"verified"} | 422 {"missing":[],"mismatched":[]}
GET   /v0/episodes/{id}               → status + missing files (resume after reconnect)
PATCH /v0/episodes/{id}               body {"outcome"}; human/eval auth, records who + when
GET   /v0/models/{model_id}/versions/{version}  → {"sha256","bytes","format","url"}  (no "latest" in v0)
```

## Sizing and cost (list price, to verify once the cloud is chosen)
| Item | Figure |
|---|---|
| One 60 s video chunk | ~15 MB per camera (single-part PUT) |
| `steps` data | negligible next to video (50 rows/s) |
| Device disk | 2 days × 8 h × 2 GB/hr = **32 GB required**; keep 64 GB free (2× headroom) |
| Storage at 100 recorded h | ~200 GB ≈ $5/mo |
| Growth at a full 8 h/day of recording | ~480 GB/mo ≈ +$11/mo each month (S3 standard) |

## Open
- Frame→tick index: per-chunk `frame_index` file or a list in the manifest? **Alex** decides.
- Pinned LeRobot format version: **Alex**, at the 30 Sep meet.
- Cloud and region: **pankaj** (setup-plan Q5 and Q6).
