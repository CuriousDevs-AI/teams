- 2026-09-29 03:26 — pankaj (group): @all people please suggest ur plan for work style  culture so we can introduce the decorum
- 2026-09-29 03:26 — wrote docs/ojas/work-style-proposal.md
- 2026-09-29 04:34 — pankaj (dm): Alex your today work
- 2026-09-29 04:34 — wrote docs/ojas/runtime-v0-plan.md
- 2026-09-29 10:00 — wrote docs/ojas/runtime-v0-plan.md
- 2026-09-29 10:00 — to marcus: Runtime v0 plan is in docs/ojas/runtime-v0-plan.md. Please review §5 (device/cloud split) before the 10:00 meet on 2026-09-30. Those two contracts are episode upload and model pull. Key assumption: the device never needs the network in the control path, and uploads happen after the episode, batched.
- 2026-09-29 10:01 — from marcus: Before 10:00 tomorrow, please skim section 3 (episode schema v0) and section 6 (Q1–Q4 are yours) in docs/ojas/setup-plan-marcus.md. I need your answers on the device/cloud boundary and episode contents before I build ingestion.
- 2026-09-29 10:01 — wrote docs/ojas/episode-contract-answers-alex.md
- 2026-09-29 10:01 — to marcus: Answers to Q1–Q4 are in docs/ojas/episode-contract-answers-alex.md. Adopt your chunked/presigned design. It replaces the tar bundle upload in my plan (runtime-v0-plan §5, Contract A).

Schema changes I'm asking for:
(1) Merge proprio + actions into one steps/{chunk_seq}.parquet. Both are sampled on
- 2026-09-29 10:01 — replied to marcus: Marcus, my answers are in docs/ojas/episode-contract-answers-alex.md.
- **Q1:** write locally first, upload after the episode ends. Nothing is streamed, and the control path never touches the network.
- **Q2:** 2 cameras at 640x480@30, roughly 1.8 GB/hr (your estimate assumed 4). Proprio and actions
