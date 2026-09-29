# Ojas — backend/infra setup plan (draft for 30 Sep planning meet)

Owner: Marcus · Status: draft for discussion · Task: T-002

## 1. Principle
We store and version what the robot records, and we don't build a streaming platform yet. The stack is Postgres plus object storage plus a single API service. Kafka, Kubernetes and real-time telemetry stay out until we have measured volume that needs them.

## 2. Proposed first tasks (each ≤5 days)

| # | Task | Owner | Size | Depends on |
|---|------|-------|------|------------|
| 1 | Repo + CI baseline: lint, tests, branch protection, one deploy path to `dev` | Marcus | 2d | Q5 (cloud choice) |
| 2 | Episode schema v0: written contract (manifest JSON + storage layout), agreed before any code | Marcus + Alex | 1–2d | Q1–Q3 |
| 3 | Ingestion v0: device requests presigned upload URLs and uploads chunks. The manifest is registered in Postgres. Retries are idempotent. | Marcus | 5d | #2, Q4 |
| 4 | Environments + secrets: `dev` and `staging` only; secrets in the cloud secret manager; no prod until a real device uploads | Marcus | 1–2d | Q5 |
| 5 | Backups: Postgres PITR on, object versioning on, first restore test logged | Marcus | 1d | #4 |

The following are out of scope for now: dataset/training pipeline, dashboards, streaming telemetry, multi-region. Each is revisited once task 3 carries real episodes.

## 3. Episode schema v0 (draft — needs Alex)

**Object storage layout** (write-once, never overwritten):
```
episodes/{device_id}/{episode_id}/manifest.json
episodes/{device_id}/{episode_id}/video/{camera_id}/{chunk_seq}.mp4
episodes/{device_id}/{episode_id}/proprio/{chunk_seq}.parquet
episodes/{device_id}/{episode_id}/actions/{chunk_seq}.parquet
```

**Postgres:**
```sql
create table devices (
  id            uuid primary key,
  name          text not null,
  hw_revision   text,
  created_at    timestamptz not null default now()
);

create table episodes (
  id              uuid primary key,          -- generated ON DEVICE, so retries are idempotent
  device_id       uuid not null references devices(id),
  runtime_version text not null,             -- Ojas runtime build
  model_version   text,                      -- policy/model that produced actions
  started_at      timestamptz not null,
  ended_at        timestamptz,
  status          text not null check (status in ('uploading','complete','failed')),
  meta            jsonb not null default '{}',
  created_at      timestamptz not null default now()
);

create table episode_files (
  episode_id  uuid not null references episodes(id),
  kind        text not null check (kind in ('video','proprio','actions','manifest')),
  stream      text not null default '',      -- camera_id for video
  chunk_seq   int  not null,
  sha256      text not null,
  bytes       bigint not null,
  uploaded_at timestamptz,
  primary key (episode_id, kind, stream, chunk_seq)
);
```

**Failure handling:**
- **Duplicate upload.** The primary key and sha256 match, so the upload is a no-op.
- **Link drops mid-episode.** The device keeps the chunks locally and resumes later. The episode stays `uploading` until every chunk listed in the manifest is present.
- **Partial upload that never finishes.** A daily job flags episodes stuck in `uploading` for more than 7 days. Nothing is auto-deleted.

## 4. Cost estimate (list prices, approximate — to verify before committing)

Assumptions: 3 cameras at roughly 3 Mbps H.264 each, so about **4 GB per recorded hour**. This is an assumption, and Q2 is where it gets confirmed or corrected.

| Item | At 100 recorded hrs (~400 GB) | At 1,000 hrs (~4 TB) |
|------|------|------|
| Object storage (S3/GCS standard, ~$0.02–0.023/GB-mo) | ~$8–10/mo | ~$80–95/mo |
| Managed Postgres (smallest tier, Mumbai) | ~$15–30/mo | same |
| API service (1 small container/VM) | ~$10–20/mo | same |
| CI (GitHub Actions free tier, if we use GitHub) | $0 | $0 |
| **Total** | **~$35–60/mo** | **~$105–145/mo** |

Egress is not included. Downloading data for training outside the cloud is the cost that will bite first, so we should train in the same cloud and region as the data.

## 5. What breaks first
Upload bandwidth from the robot. Video can easily exceed what a site uplink carries. We may need on-device compression/downsampling or selective upload, and that decision belongs in the runtime (Alex/Ethan), not the backend.

## 6. Open questions

**Alex**
- Q1. Where is the device/cloud boundary? Does the runtime write episodes locally first (my assumption) or stream them?
- Q2. What does an episode contain: cameras, resolution/fps, proprioception rate, action format?
- Q3. Do we record every run, or only flagged or failed ones?

**Ethan (via Alex if needed)**
- Q4. What is the realistic uplink at the robot's site, and how much local disk is on the device?

**pankaj**
- Q5. AWS or GCP? Do we have credits or existing accounts anywhere? My default is whichever gives credits, in the Mumbai region. Creating accounts and spending money is 🔴, so I will ask for approval before doing either.
- Q6. What is the monthly spend ceiling for Ojas infra over the next 3 months?
