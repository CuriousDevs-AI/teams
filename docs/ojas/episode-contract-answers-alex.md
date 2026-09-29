# Episode contract — Alex's answers to Marcus (Q1–Q4)

Written 2026-09-29. It responds to `docs/ojas/setup-plan-marcus.md` §3 and §6.

This **supersedes Contract A in `docs/ojas/runtime-v0-plan.md` §5**. Marcus's chunked, presigned upload design replaces my tar bundle.

All rates below are **v0 targets**. The final numbers come once pankaj confirms the hardware (cameras, arm, Jetson).

## Q1 — Device/cloud boundary
- **Write locally first, then upload. No streaming in v0.**
- The runtime writes chunks to local NVMe while the episode runs.
  - Chunks rotate every **60 s**, so a crash loses at most one chunk.
- Upload starts **after the episode ends**, and only while the robot is idle. It never runs during policy execution, so it doesn't compete with inference for CPU, GPU or IO.
- The control path never depends on the network. The device runs fully offline and uploads whenever it gets the chance.
- The device deletes a local chunk only after the server confirms it **and** the sha256 matches. The episode also has to reach `complete`.

## Q2 — What an episode contains

| Stream | v0 target | Format |
|---|---|---|
| Cameras | **2** (wrist + scene), 640×480 @ 30 fps | H.264 mp4, 60 s chunks, ~1.5–2 Mbps each |
| Proprioception | 50 Hz (control rate): joint pos, joint vel, effort (if the driver exposes it), gripper state | steps parquet |
| Action — raw | Model or teleop output **before** the safety envelope | steps parquet |
| Action — commanded | What was actually sent to hardware **after** the safety envelope, plus a `clamped` bool | steps parquet |
| Events | Safety violations, fallback transitions, model-chunk arrivals, teleop takeovers | `events.jsonl`: `{ts, type, cause, action_taken}` |

**Volume:** 2 cameras × ~2 Mbps ≈ **1.8 GB per recorded hour**. Use **2 GB/hr** for planning, not 4.

### Why log both raw and commanded actions
The gap between them is how we see where the model is unsafe. It is also the most useful signal for fine-tuning and evals. Logging only one of them loses that.

### Schema changes requested

**1. Merge proprio and actions into one file.** Use `steps/{chunk_seq}.parquet`, with one row per control tick. Both streams are sampled on the same tick, so separate files only create a join problem later. Columns:

```
tick           int64     -- monotonic control tick
ts_ns          int64     -- device monotonic clock
obs_state      list<float32>
action_raw     list<float32>
action_cmd     list<float32>
clamped        bool
mode           string    -- teleop|policy|hold|stop
```

**2. Add `events` to `episode_files.kind`.**

**3. Add columns to `episodes`:**
- `task text not null`
- `mode text` (teleop|policy|mixed)
- `outcome text` (success|fail|aborted|unlabelled; default 'unlabelled')
- `control_hz int`

Keep `outcome` separate from the upload `status`. They are different things.

**4. Manifest contents.** The manifest lists every expected chunk with its sha256 and byte count. It also records:
- camera ids and their resolution/fps
- the clock source
- the pinned LeRobot format version

### Timing and conversion
- **Timestamps:** use one device monotonic clock. Map each video frame to a tick within ≤ 10 ms. Store that frame→tick mapping in the manifest or in a small per-chunk index. I'll spec it in the runtime.
- **Conversion to LeRobotDataset:** this is a cloud-side batch job, done later. Raw storage stays in the layout above, so ingestion doesn't depend on the LeRobot version.

## Q3 — Record every run?
- **Yes: every run in v0**, covering teleop, policy and failed runs.
- Volume will be small at first. Failures and teleop takeovers are the most valuable data, and we can't tell in advance which runs matter.
- We revisit selective upload when either of these happens:
  - upload backlog exceeds 24 h, or
  - storage passes the Q6 spend ceiling.

## Q4 — Uplink and disk
- **The v0 device is the Ojas dev rig** (arm + Jetson in the office). It is not a Parth site. The uplink is office wifi/ethernet, so it isn't a constraint for v0.
- **Disk rule:** the device must hold **≥ 2 days** of recording without uploading. At 8 h/day × 2 GB/hr ≈ 32 GB, that means **≥ 64 GB of free NVMe** on the Jetson. This is pending the hardware pankaj confirms.
- **Parth site uplink:** a question for Ethan, once Parth runs on real sites. Your point in §5 ("what breaks first") stands. When that time comes, on-device downsampling or selective upload belongs in the runtime, and I'll own it.

## Still open (not mine)
- Q5 (AWS vs GCP) and Q6 (spend ceiling) belong to pankaj.
- On the hardware (Jetson model, cameras, arm): I need pankaj to confirm at 10:00 on 2026-09-30. It sets the final camera count and disk numbers.
