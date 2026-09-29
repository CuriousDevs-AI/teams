# Ojas Runtime v0 — plan

Owner: Alex · Task: T-003 · Written: 2026-09-29 · Status: draft for review at the 10:00 meet on 2026-09-30

**Rule for this doc:** every number here is a **target to measure**, not a result. Nothing has been benchmarked yet.

## 1. Scope

**Goal of v0:** a model can drive one robot through one task. A deterministic loop owns timing and safety, and every run is logged as a trainable episode.

In v0:
- One robot, one manipulation task (e.g. pick-and-place of one object class).
- Teleop data collection → fine-tune an open small policy → run it through the runtime → 20-trial eval.
- A safety envelope and a fallback ladder that do not depend on the model behaving.
- Telemetry and episode logging on every run, including failures.

Not in v0:
- Multiple robots or hardware targets.
- Our own model.
- Cloud inference.
- Humanoid/Parth integration. That starts after we have a v0 eval number.
- A web UI. Sofia is not needed on the runtime path yet; a dashboard comes later.

### Build vs reuse (decision)
Don't rebuild what exists:
- **LeRobot (Hugging Face).** We use it for teleop, the dataset format, and training/inference for ACT, SmolVLA and pi0-family policies.
- **ROS 2 + ros2_control.** We use it for the hardware interface, where a driver exists.

**What we build (our value):**
- The control-loop timing guarantees.
- The safety envelope and fallback ladder.
- Model-version pinning.
- Telemetry, and eval tooling across repeated trials.

## 2. Hardware target

| Target | v0 role |
|---|---|
| Jetson Orin (NX 16GB or AGX — **assumption, pankaj to confirm what we own**) | On-robot inference + control loop. Primary target. |
| x86 + NVIDIA GPU | Dev, training, sim, and inference fallback if the Orin budget fails. |
| Raspberry Pi | **Not an inference target in v0.** At most an IO/motor-bus node. |

Robot: **open question.** We need to know what arm or robot we have. If we have nothing, a LeRobot-native low-cost arm (SO-100/SO-101 class) is the fastest path. Buying one is 🔴 (money), so it needs pankaj's call.

## 3. Architecture (processes)

```
cameras/joints ──► [sensor node] ──► obs buffer (timestamped)
                                         │
                   [inference worker] ◄──┘   async, 2–10 Hz, outputs action CHUNKS
                         │
                         ▼
                  [action queue] ──► [control loop 50 Hz] ──► [safety envelope] ──► hardware
                                              ▲                        │
                          teleop override ────┘                        ▼
                                                              [logger: episode + telemetry]
```

- The control loop never waits on inference. It consumes a queued action chunk.
- Teleop override always wins, even over a valid model output.

## 4. Latency budget (targets)

| Item | Target |
|---|---|
| Control loop period | 20 ms (50 Hz) |
| Control tick jitter | p99 < 2 ms |
| Safety envelope check per tick | < 0.5 ms |
| Inference per chunk (on Orin) | p99 < 300 ms — **unmeasured; the first thing we benchmark** |
| Action chunk length | ~1 s (50 steps) |
| Max observation age at action time | 500 ms, else fallback |
| Refill trigger | when < 300 ms of chunk remains |

### Safety envelope (every tick, model-independent)
- Joint position, velocity and acceleration limits.
- A workspace bounding box.
- A NaN/inf check.
- A max step delta between consecutive actions.
- A watchdog on both the control loop and the inference worker.
- The e-stop is hardware, not software.

### Fallback ladder
1. The new chunk is late → keep executing the remaining queued chunk.
2. The queue is empty or the observation is too old → **hold position**.
3. Hold lasts > 2 s, or the envelope is violated → **controlled stop** and hand to teleop.
4. Every fallback event is logged with its cause.

## 5. Device / cloud split — contract for Marcus

**Rule:** nothing in the control path touches the network. The device runs fully offline.

| On device | In cloud (Marcus) |
|---|---|
| Sensors, inference, control, safety, logging | Episode storage, dataset versioning, dedup |
| Local episode buffer (disk) | Training jobs, model registry |
| Model artifact cache | Eval result store |

### Contract A — episode upload
- Uploads happen after each episode, batched, and are resumable.
- Unit: one episode bundle, sent as a `.tar` with:
  - `meta.json`
  - LeRobot-format parquet (states/actions)
  - `mp4` per camera
  - `events.jsonl` (safety and fallback events)
- The device deletes its local copy only after the server confirms the upload **and** the checksum matches.

### Contract B — model pull
- The device pulls a model artifact by `model_id@version`, with a sha256.
- The runtime refuses to load an artifact that is unpinned or has a mismatched hash.

### `meta.json` (draft)
```json
{
  "episode_id": "uuid",
  "robot_id": "string",
  "task": "string",
  "runtime_version": "semver",
  "model_id": "string|null",
  "model_version": "string|null",
  "mode": "teleop|policy|mixed",
  "start_ts": "ISO8601",
  "end_ts": "ISO8601",
  "control_hz": 50,
  "outcome": "success|fail|aborted|unlabelled",
  "fallback_count": 0,
  "safety_violations": 0,
  "sha256": {"file": "hash"}
}
```

## 6. Episode format

- Base: the **LeRobotDataset** format. We pin the exact version at project start. We do not invent our own.
- Our additions:
  - `events.jsonl`, with one line per safety or fallback event: `{ts, type, cause, action_taken}`.
  - Runtime and model version in `meta.json`.
  - An outcome label.
- Timestamps come from one monotonic clock on the device. Camera frames are matched to joint state within 10 ms.

## 7. Eval (v0)

- One task, **20 trials** per condition, fixed reset procedure.
- Conditions: teleop baseline vs policy.
- Report:
  - success rate (n=20)
  - mean time to complete
  - fallback events per trial
  - p50/p99 inference latency
  - control jitter

## 8. Milestones (each ≤ 5 days, will become board tasks after review)

| By | Deliverable | Measured by |
|---|---|---|
| 2026-10-06 | Control loop + safety envelope + logging, teleop only, no model | Jitter p99 measured; 10 teleop episodes recorded in the format above |
| 2026-10-13 | Policy (ACT or SmolVLA) fine-tuned on the teleop data, running through the runtime | Inference p50/p99 on target hardware measured |
| 2026-10-20 | 20-trial eval + first episode upload to Marcus's store | Eval table in `docs/ojas/`, bundles verified server-side |

## 9. Decisions needed at 10:00 on 2026-09-30

1. **Hardware on hand:** which Jetson (if any), which GPU box, which robot or arm. This blocks milestone 1.
2. **The task:** which single task we demo.
3. **Marcus:** does the Contract A/B shape work for storage? And where do the bundles live?
4. **Robot purchase:** if we have no robot, approve a low-cost arm purchase (🔴).
