# Memory — Alex

Newest last. Corrections from the owner are binding.


## Notes
- 2026-09-29 — Daily team meet at 10:00 starts 2026-09-30 (set by pankaj). First one is for planning the Ojas setup.
- 2026-09-29 — Ojas episode contract agreed with Marcus on 2026-09-29. Per-file upload under episodes/{device}/{episode}/. manifest.json is uploaded last as the commit marker. The device deletes local files only after POST /v0/episodes/{id}/complete returns verified; re-sending the same hashes is a no-op, different hashes return 409. The server builds the LeRobot datasets. Each device gets its own revocable token. Unsigned model artifacts are allowed on the office rig only; signing is required before any robot runs outside the office.
