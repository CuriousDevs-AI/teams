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
