# Review of runtime-v0 §5 — Contract A (episode upload) & B (model pull)

Reviewer: Marcus · 2026-09-29 · Reviews: docs/ojas/runtime-v0-plan.md §5
Status: proposal. Alex must confirm before either side builds.

## Verdict
The split is right. I agree with keeping the network out of the control path and with batched post-episode upload. I propose four changes, listed below.

## Change 1 — upload per file, not one .tar
- **Why.** The server can check each file's sha256 on arrival without unpacking a tar. A dropped link also only re-sends the missing files.
- **What stays the same.** The contents are unchanged: `meta.json`, parquet, one mp4 per camera, `events.jsonl`.
- **Commit marker.** `meta.json` is uploaded last. Until the server has verified every file, the episode stays `uploading` and is not used for training.

Storage layout (write-once):
```
episodes/{robot_id}/{episode_id}/meta.json
episodes/{robot_id}/{episode_id}/data/*.parquet
episodes/{robot_id}/{episode_id}/video/{camera_id}.mp4
episodes/{robot_id}/{episode_id}/events.jsonl
```

## Change 2 — explicit verify step, idempotent
The device deletes its local copy **only** when the response is `verified`.

## Change 3 — the server builds datasets
- The device uploads per-episode files.
- The server assembles versioned LeRobot datasets from episodes that are `verified` and match a filter.
- **To confirm at the meet:** the exact LeRobot version we pin. I believe newer versions pack multiple episodes per file. If so, the device must still write per-episode files, or an export step must run before upload.

## Change 4 — per-robot auth from day one
- Each robot gets its own bearer token. It is stored hashed in Postgres and can be revoked individually. There is no shared key.
- mTLS can come later.
- Artifacts are hash-checked but **not signed** in v0. That is a logged decision, to be revisited before any robot leaves our lab.

## Proposed `meta.json` additions
- `schema_version`: `"ojas-episode/0"`
- `lerobot_format_version`
- `sha256`: covers every file except `meta.json` itself

## Endpoints (v0)
Error format for all: `{"error":{"code":"string","message":"string","details":{}}}`

### Contract A
```
POST /v0/episodes
  body: {"episode_id":"uuid","robot_id":"str",
         "files":[{"path":"data/episode.parquet","sha256":"hex","bytes":123}]}
  201/200: {"episode_id":"uuid","status":"uploading",
            "uploads":[{"path":"...","url":"<presigned PUT, 1h>","headers":{...}}]}
  409: episode_id exists with different file hashes (nothing overwritten)

PUT <presigned url>              # one per file; retry-safe

POST /v0/episodes/{id}/complete
  200: {"status":"verified"}
  422: {"error":{"code":"incomplete","details":{"missing":[],"mismatched":[]}}}

GET /v0/episodes/{id}            # status check after reconnect
```

### Contract B
```
GET /v0/models/{model_id}/versions/{version}
  200: {"model_id":"str","version":"str","sha256":"hex","bytes":123,
        "format":"str","url":"<presigned GET, 15 min>"}
  404: unknown id/version (the runtime must refuse; no "latest" alias in v0)
```
A `model_id@version` is immutable once published.

## Failure modes covered
| Case | Behaviour |
|---|---|
| Link drops mid-upload | `GET /v0/episodes/{id}` returns what is missing; the device re-PUTs only those files |
| Duplicate episode_id, same hashes | no-op; `/complete` returns `verified` |
| Duplicate episode_id, different hashes | 409; both copies stay on the device for inspection |
| Server loses a file before verify | `/complete` returns 422 with `missing`; the device still holds its copy |
| Episode stuck `uploading` > 7 days | flagged daily; never auto-deleted |

## Open / needs decision
- Cloud and region: Mumbai. AWS vs GCP is pankaj's call (setup-plan Q5, 🔴 for account and spend).
- Pinned LeRobot version: Alex, at the meet.
- Typical episode length and camera count: Alex. This drives the cost estimate in setup-plan §4.
