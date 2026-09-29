---
id: T-003
title: 'Runtime v0 plan: scope, hardware target, latency budget, device/cloud split,
  episode format'
owner: alex
project: ojas
priority: P1
status: review
assigned: '2026-09-29'
due: '2026-09-30'
depends_on: []
reviewer: pankaj
goal: ''
created_by: pankaj
blocked_on: ''
status_since: '2026-09-29'
updated: '2026-09-29'
---

## Goal
One-page plan for Ojas runtime v0. Draft written 2026-09-29; to be finalised at the first daily meet.

## Done means
- [ ] docs/ojas/runtime-v0-plan.md reviewed in 2026-09-30 10:00 meeting
- [ ] Hardware target fixed from what we actually own
- [ ] Episode format agreed with Sofia and Marcus
- [ ] v0 split into <=5-day tasks on the board

## Log
- 2026-09-29 — alex: Contract converged into Marcus's docs/ojas/episode-contract-v0.md; my answers doc is superseded. Disk requirement corrected to 32 GB (64 GB is headroom). frame_index is a per-camera, per-chunk parquet (frame_idx, pts_ns, tick). Proposed outcome_source.
- 2026-09-29 — alex: Marcus's review of Contract A/B accepted: per-file upload, verified-complete before local delete, server-side LeRobot assembly, per-device tokens. manifest.json is the single commit marker. Unsigned artifacts allowed on the office rig only; signing required before external deployment.
- 2026-09-29 — alex: Answered Marcus's Q1–Q4. Adopted his chunked upload design over my tar bundle. Schema deltas sent to Marcus.
- 2026-09-29 — alex: review
- 2026-09-29 — alex: Full v0 plan written: scope, hardware, latency budget, fallback ladder, device/cloud contract, episode format, 3-week milestones. Hardware choice is an assumption (Orin) pending pankaj.
- 2026-09-29 — created by pankaj, assigned to alex

## Feedback

## Output
- 2026-09-29 — alex: docs/ojas/runtime-v0-plan.md
- 2026-09-29 — alex: docs/ojas/episode-contract-answers-alex.md
- 2026-09-29 — alex: docs/ojas/runtime-v0-plan.md, docs/ojas/episode-contract-answers-alex.md
- 2026-09-29 — alex: docs/ojas/runtime-v0-plan.md, docs/ojas/episode-contract-answers-alex.md, docs/ojas/contract-ab-review-marcus.md
- 2026-09-29 — alex: docs/ojas/runtime-v0-plan.md, docs/ojas/episode-contract-v0.md
