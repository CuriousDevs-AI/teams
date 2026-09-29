# Ojas episode contract v0 (canonical)

Owners: Marcus (storage/API) + Alex (runtime). Date: 2026-09-29.
Status: **Confirmed by Alex (frame_index, corrections 1–3, derived-tick rules a–b). One item pending: Alex's ack on the manifest fields `start_mono_ns` / `end_mono_ns`, or the fallback. Due 2026-09-29 18:00.**

Supersedes:
- `setup-plan-marcus.md` §3
- `contract-ab-review-marcus.md` §Contract A
- `runtime-v0-plan.md` §5 Contract A

Inputs: `episode-contract-answers-alex.md`; Alex in #Ojas and inbox on 2026-09-29 (frame_index, ts-only, bframes=0, pipeline checks); derived-tick edge cases from James via Alex on 2026-09-29.

## Rules
- The device writes locally, in 60 s chunks. Upload starts after the episode ends and only while the robot is idle. Nothing in the control path touches the network.
- The device deletes its local files **only** after `POST /v0/episodes/{id}/complete` returns `{"status":"verified"}`.
- Objects are write-once. An `episode_id` is generated on the device.
- Planning volume is **2 GB per recorded hour** (2 cameras, 640×480@30, about 2 Mbps each).
- Every run is recorded in v0.
- **Video encoding: `bframes=0` (device requirement).** Decode order must equal presentation order. The pipeline rejects a chunk whose ffprobe reports B-frames.
- Device writers store only raw observations. Derived values (e.g. frame→tick) are computed in the pipeline.

## Storage layout
```
episodes/{robot_id}/{episode_id}/manifest.json
episodes/{robot_id}/{episode_id}/steps/{chunk_seq}.parquet
episodes/{robot_id}/{episode_id}/video/{camera_id}/{chunk_seq}.mp4
episodes/{robot_id}/{episode_id}/frame_index/{camera_id}/{chunk_seq}.parquet
episodes/{robot_id}/{episode_id}/events.jsonl
```

## `steps` parquet (one row per control tick)
```
tick int64 · ts_ns int64 · obs_state list<float32> · action_raw list<float32>
action_cmd list<float32> · clamped bool · mode string (teleop|policy|hold|stop)
```
- **`tick` is a global monotonic counter per episode, starting at 0.** It does not reset per chunk. Chunk k+1 continues from the last tick of chunk k.
- The per-tick `mode` is not the same as the episode-level `mode`. The two use different enums on purpose.

## `frame_index` parquet (one per camera per chunk)
Each file covers the mp4 with the same `camera_id` and `chunk_seq`. It has one row per frame actually encoded in that mp4:
```
frame_idx int64   -- 0-based within the chunk; the frame's position in the decoded (presentation) output of the mp4
ts_ns     int64   -- capture time, device_monotonic (same clock as steps.ts_ns)
```
- Dropped frames: no row. A drop shows up as a gap in `ts_ns`.
- About 1,800 rows per camera-chunk (under 50 KB).
- In the manifest `files`, each index is listed with `kind: "frame_index"`, `stream: <camera_id>`, and the same `chunk_seq` as the matching video.
- The frame_index lives here rather than in the manifest because a manifest list would be ~216k entries/hr, and the manifest is the commit marker that is uploaded last.

## Derived tick (pipeline only)
- tick(frame) = the `steps.tick` of the latest step with `steps.ts_ns <= frame.ts_ns`, computed as an as-of join.
- **The join runs over all of the episode's steps, concatenated in `chunk_seq` order, not per chunk.** Video and steps chunk boundaries are not aligned, so a frame can map to a tick in the previous steps chunk.
- **Frames captured before the first control tick get tick = null.** They stay in the video and in frame_index, and are excluded from training rows.
- The device never writes the tick into frame_index.

## Pipeline checks (dataset build, not `/complete`)
`/complete` verifies only sha256 and bytes. The dataset pipeline checks:
1. For each frame_index/mp4 pair, row count = the mp4's decoded frame count.
2. `frame_idx` is contiguous, 0..n-1, within each chunk.
3. `ts_ns` is strictly increasing within each chunk and across chunks, per camera.
4. `ts_ns` is within `[start_mono_ns, end_mono_ns]` from the manifest. *(Fallback if Alex rejects the fields: within `[min(steps.ts_ns) − 1 s, max(steps.ts_ns) + 1 s]`.)*
5. The mp4 has no B-frames.
6. `steps.tick` is contiguous, 0..N-1, across all steps chunks in `chunk_seq` order. `steps.ts_ns` is strictly increasing.

Metric: drop rate per camera per episode, computed from `ts_ns` gaps against the nominal fps.

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
  "start_mono_ns": 0,
  "end_mono_ns": 0,
  "state_names": ["j1_pos", "..."],
  "action_names": ["j1", "..."],
  "cameras": [{"id": "wrist", "width": 640, "height": 480, "fps": 30}],
  "files": [
    {"kind": "steps", "stream": "", "chunk_seq": 0, "path": "steps/0.parquet", "sha256": "hex", "bytes": 123},
    {"kind": "video", "stream": "wrist", "chunk_seq": 0, "path": "video/wrist/0.mp4", "sha256": "hex", "bytes": 15000000},
    {"kind": "frame_index", "stream": "wrist", "chunk_seq": 0, "path": "frame_index/wrist/0.parquet", "sha256": "hex", "bytes": 40000}
  ]
}
```
- `start_ts` and `end_ts` are wall-clock times, for humans and for queries. `start_mono_ns` and `end_mono_ns` are device_monotonic, sampled at the same moments, and are used for checks against `ts_ns`. *(Pending Alex's ack.)*
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
  manifest               jsonb not null,           -- includes start_mono_ns/end_mono_ns
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
| `frame_index` data | ~1,800 rows per camera-chunk, negligible |
| Device disk | 2 days × 8 h × 2 GB/hr = **32 GB required**; keep 64 GB free (2× headroom) |
| Storage at 100 recorded h | ~200 GB ≈ $5/mo |
| Growth at a full 8 h/day of recording | ~480 GB/mo ≈ +$11/mo each month (S3 standard) |

## Decided
- 2026-09-29: frame→tick uses a per-camera per-chunk `frame_index` parquet with columns `frame_idx` and `ts_ns`. The tick is derived in the pipeline.
- 2026-09-29: `bframes=0` is a device encoding requirement. `frame_idx` is in presentation order.
- 2026-09-29: `steps.tick` is a global counter per episode, starting at 0. The as-of join spans all chunks. Frames before the first tick get tick = null and are excluded from training.

## Known gaps (v0.1, not blocking)
- frame_index records capture time, not which frame the policy consumed at tick t (perception latency). `steps` will need `obs_frame_idx` per camera. Alex raises this at the 30 Sep meet.

## Open
- `start_mono_ns` / `end_mono_ns`: **Alex** to reply ack or fallback, 2026-09-29 18:00.
- Pinned LeRobot format version: **Alex**, at the 30 Sep meet.
- Cloud and region: **pankaj** (setup-plan Q5 and Q6).
