# Memory — Alex

Newest last. Corrections from the owner are binding.


## Notes
- 2026-09-29 — Daily team meet at 10:00 starts 2026-09-30 (set by pankaj). First one is for planning the Ojas setup.
- 2026-09-29 — Ojas episode contract agreed with Marcus on 2026-09-29. Per-file upload under episodes/{device}/{episode}/. manifest.json is uploaded last as the commit marker. The device deletes local files only after POST /v0/episodes/{id}/complete returns verified; re-sending the same hashes is a no-op, different hashes return 409. The server builds the LeRobot datasets. Each device gets its own revocable token. Unsigned model artifacts are allowed on the office rig only; signing is required before any robot runs outside the office.
- 2026-09-29 — The canonical Ojas episode contract is docs/ojas/episode-contract-v0.md (Marcus). Upload status is uploading|verified. The manifest has state_names and action_names. Outcome is set via PATCH /v0/episodes/{id} (plus an outcome_source field I proposed). The frame→tick mapping is a per-camera, per-chunk frame_index parquet (frame_idx, pts_ns, tick). Device disk requirement: 32 GB for 2 days at 2 GB/hr; 64 GB is headroom. Separate requirement from margin in future estimates.
- 2026-09-29 — Lesson from T-003: write the interface draft early, with assumptions stated, then let the implementing side (Marcus) own the canonical contract file. Adopt their better design quickly and mark my own doc as superseded. The episode contract converged in one day this way.
- 2026-09-29 — Lesson from T-003: in every plan, label each number as a requirement, headroom/margin, or target-unmeasured. I conflated 32 GB (requirement) with 64 GB (headroom) and Marcus caught it.
- 2026-09-29 — 2026-09-29 — frame_index final (agreed with Marcus): frame_index/{camera_id}/{chunk_seq}.parquet with columns frame_idx int64 (presentation order, = position in decoded mp4) and ts_ns int64 (capture, device_monotonic). There is NO tick column. The pipeline derives tick with an as-of join on steps.ts_ns; pre-first-tick frames get null. The device encoder uses bframes=0. Pipeline checks: row count = mp4 frame count, frame_idx contiguous, ts_ns strictly increasing and inside the episode window. Supersedes my earlier (frame_idx, pts_ns, tick) note. Known gap for v0.1: steps.obs_frame_idx per camera (the frame the policy actually consumed).
