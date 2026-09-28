# Ojas runtime v0 — plan (DRAFT, 2026-09-29)

Owner: Alex · Review: 2026-09-30 10:00 daily meet
Status: draft. **No numbers in this doc are measured yet.** Every figure is a target.

## 1. What v0 is
v0 is the smallest runtime that can be measured.

- It takes one camera stream and robot state as input.
- It runs one open policy.
- It passes every action through a safety gate before it reaches the controller.
- It logs every episode.
- It has one fallback: hold position, or teleop.

Out of scope for v0:
- multi-robot setups
- cloud inference
- fine-tuning
- our own model

## 2. Don't rebuild what exists
- **Control / hardware I/O:** ROS 2 (Jazzy) + ros2_control. We do not write our own control loop framework.
- **Episode format + training tooling:** LeRobot (LeRobotDataset: parquet + mp4). We do not invent a format.
- **Inference:** ONNX Runtime / TensorRT on Jetson, and PyTorch on x86 for development.
- **What Ojas adds on top:**
  - the safety gate
  - the deadline and fallback logic
  - telemetry
  - a stable API for Parth

## 3. Hardware target
**Open.** It depends on what we own (question for pankaj).

- Default target: Jetson Orin (NX or AGX) for the device, plus an x86 + GPU box for development.
- If we have no arm, v0 runs in MuJoCo sim with a cheap arm model (SO-100 class). We say that plainly in any demo.

## 4. Loop and latency budget (targets)
| Stage | Rate | Budget (p99) |
|---|---|---|
| Low-level control (ros2_control) | 50 Hz | 20 ms period, jitter < 2 ms |
| Camera capture + preprocess | 10 Hz | ≤ 15 ms |
| Policy inference (small VLA / ACT, action chunk) | 5–10 Hz | ≤ 80 ms on Orin — **to be measured** |
| Safety gate | per action | ≤ 1 ms |

**Deadline miss:**
- Keep executing the remaining actions in the current chunk for up to N steps.
- After that, hold position and raise an alert.

**Garbage output** (NaN, out of range, joint limit violated, velocity or acceleration over its cap):
- reject the output
- hold position
- log the failure as a labelled event

## 5. Episode / telemetry format (for Sofia and Marcus)
We adopt LeRobotDataset. Each episode contains:

- **Synced video:** mp4, one file per camera.
- **Per-step table (parquet), one row per step:**
  - `timestamp`
  - `observation.state`
  - `action`
  - `policy_latency_ms`
  - `safety_verdict`: one of pass / clamp / reject
  - `fallback_active`
- **Episode metadata:**
  - `episode_id`
  - `task`
  - `robot_id`
  - `runtime_version`
  - `model_id`
  - `outcome`: one of success / fail / aborted
  - `failure_reason`

Live telemetry is the same fields, streamed per step. The first dashboard screen should be episode browsing, because that data exists from day 1.

## 6. Device / cloud split (with Marcus)
**On device:**
- inference
- the safety gate
- the control loop
- local episode buffer

**Cloud:**
- episode upload
- storage and versioning
- dedup
- the dashboard API

**Rule:** the robot never depends on the network to act safely.

## 7. Milestones (evidence-gated)
1. **M1:** the policy runs in a loop on the target, and p50/p99 latency is measured over 1,000 steps.
2. **M2:** the safety gate and fallback are tested using injected bad outputs. The pass criterion is 100% of injected faults caught.
3. **M3:** 50 logged episodes on one real or sim task, with a success rate over those 50.
4. **Next stage** (fine-tuning): only after M3 numbers exist.

## 8. Open questions for 2026-09-30
- What hardware do we own right now?
- Which first task? (Suggestion: pick and place with one object.)
- Is the Parth timeline asking for runtime API v0 by a specific date? (Question for Ethan.)
