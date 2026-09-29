- 2026-09-29 03:26 — wrote docs/ojas/work-style-marcus.md
- 2026-09-29 03:30 — pankaj (group): @marcus @alex @sofia ready for tommarow daily meet @ 10.00 am , fo planning out steup
- 2026-09-29 10:00 — wrote docs/ojas/setup-plan-marcus.md
- 2026-09-29 10:00 — to alex: Before 10:00 tomorrow, please skim section 3 (episode schema v0) and section 6 (Q1–Q4 are yours) in docs/ojas/setup-plan-marcus.md. I need your answers on the device/cloud boundary and episode contents before I build ingestion.
- 2026-09-29 10:00 — from alex: Runtime v0 plan is in docs/ojas/runtime-v0-plan.md. Please review §5 (device/cloud split) before the 10:00 meet on 2026-09-30. Those two contracts are episode upload and model pull. Key assumption: the device never needs the network in the control path, and uploads happen after the episode, batched.
- 2026-09-29 10:01 — wrote docs/ojas/contract-ab-review-marcus.md
- 2026-09-29 10:01 — replied to alex: @Alex, I've reviewed §5. The shape works, with 4 changes, all written up in docs/ojas/contract-ab-review-marcus.md.
1) Upload per file under the episode prefix instead of one .tar. Contents stay exactly as you listed; `meta.json` goes last as the commit marker. This way the server verifies each file
- 2026-09-29 10:01 — from alex: Answers to Q1–Q4 are in docs/ojas/episode-contract-answers-alex.md. Adopt your chunked/presigned design. It replaces the tar bundle upload in my plan (runtime-v0-plan §5, Contract A).

Schema changes I'm asking for:
(1) Merge proprio + actions into one steps/{chunk_seq}.parquet. Both are sampled on
- 2026-09-29 10:02 — wrote docs/ojas/episode-contract-v0.md
- 2026-09-29 10:02 — replied to alex: @Alex, I accept all 4 changes and the 2 GB/hr figure. Your math checks out: 2 cams × 2 Mbps ≈ 1.8 GB/hr. The canonical contract is now in docs/ojas/episode-contract-v0.md.
One pushback and four additions:
• Disk: 8 h × 2 GB/hr × 2 days = 32 GB, not 64. Keeping 64 GB free is fine as 2× headroom, but
- 2026-09-29 10:02 — from alex: Alex replied: Marcus, my answers are in docs/ojas/episode-contract-answers-alex.md.
- **Q1:** write locally first, upload after the episode ends. Nothing is streamed, and the control path never touches the network.
- **Q2:** 2 cameras at 640x480@30, roughly 1.8 GB/hr (your estimate assumed 4). Propr
- 2026-09-29 10:02 — replied to alex: @Alex, this is the same set of answers I already handled. All 4 schema changes and the 2 GB/hr figure are in docs/ojas/episode-contract-v0.md. The only thing I changed is the disk figure: 32 GB is the requirement and 64 GB is headroom. Still waiting on you: the frame→tick index format, and confirmat
- 2026-09-29 11:06 — from james: Marcus, Alex proposed a frame→tick mapping in the #Ojas room: a separate parquet frame_index file, one per camera per chunk. Please confirm it or push back by 18:00 today so episode-contract-v0 is closed before the 10:00 meeting on 30 Sep.
- 2026-09-29 11:06 — wrote docs/ojas/episode-contract-v0.md
- 2026-09-29 11:06 — to alex: Alex, I accept the per-chunk frame_index. It's in docs/ojas/episode-contract-v0.md. Your #Ojas message was cut off at the column list, so please confirm or correct what I wrote before 18:00:

`frame_index/{camera_id}/{chunk_seq}.parquet`, one row per frame actually encoded in that mp4, in decode ord
- 2026-09-29 11:06 — replied to james: James, confirmed. The per-chunk `frame_index` parquet (one per camera per chunk) is now in docs/ojas/episode-contract-v0.md, and the open item is closed.

One caveat: Alex's message in #Ojas is cut off at the column list, so I wrote down the columns I need: `frame_idx int64` (0-based within the chun
- 2026-09-29 11:07 — from alex: Alex replied: Marcus, confirmed, with 3 corrections. I'm taking your ts-only option.

**1. Drop `tick` from frame_index. The pipeline derives it.**
If the device writes `tick`, the camera writer has to read control-loop state, which adds a cross-thread dependency on the hot path. It also stores a de
- 2026-09-29 11:07 — wrote docs/ojas/episode-contract-v0.md
- 2026-09-29 11:07 — to alex: Alex, I've applied all three corrections to docs/ojas/episode-contract-v0.md.

One addition needs your ack. Check 3 compares ts_ns (device_monotonic) against start_ts/end_ts, which are ISO8601 wall-clock. Those are different clocks, so I added two manifest fields: `start_mono_ns` and `end_mono_ns` (
- 2026-09-29 11:07 — to james: James, the frame→tick mapping is closed. It's a per-camera per-chunk frame_index with columns frame_idx and ts_ns, and the tick is derived in the pipeline. Alex confirmed, and his 3 corrections are applied in docs/ojas/episode-contract-v0.md.

One small item is still open, due 18:00: Alex needs to a
- 2026-09-29 11:07 — replied to alex: Alex, I've accepted all three corrections, and they're now in `docs/ojas/episode-contract-v0.md`:
1. `tick` is dropped. frame_index is now `frame_idx` + `ts_ns` only, and the pipeline derives the tick with an as-of join.
2. `frame_idx` is presentation order. `bframes=0` is now a device requirement,
- 2026-09-29 11:07 — from alex: Marcus, two edge-case rules for the derived tick, raised by James. Please add both to docs/ojas/episode-contract-v0.md. (a) Frames captured before the first control tick get derived tick = null. They stay in the video and are excluded from training rows. (b) Tick is a global monotonic counter per ep
