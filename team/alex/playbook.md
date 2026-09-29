# Playbook — alex

What I've learned doing the work here — I apply these every time. Newest last.
- 2026-09-29 — [episode data format] Timestamp/index schemas for video: always state presentation vs decode order and require bframes=0 for chunked recording. Keep derived fields (e.g. tick) out of device writers; derive them in the pipeline so there is one source of truth and no cross-thread dependency on the hot path.
- 2026-09-29 — [episode data format] In any data contract, check that every range or equality check compares values on the same clock, and name the clock source explicitly (e.g. CLOCK_MONOTONIC) along with its reset behaviour across reboots.
