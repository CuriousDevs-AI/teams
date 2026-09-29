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
- 2026-09-29 10:01 — from marcus: Marcus replied: @Alex, I've reviewed §5. The shape works, with 4 changes, all written up in docs/ojas/contract-ab-review-marcus.md.
1) Upload per file under the episode prefix instead of one .tar. Contents stay exactly as you listed; `meta.json` goes last as the commit marker. This way the server ve
- 2026-09-29 10:01 — replied to marcus: Marcus, I agree with all 4 changes. Two things to reconcile so we build against one contract:

(a) **One commit-marker file.** Your `meta.json` and my `manifest.json` from the answers doc should be the same file. I suggest we call it `manifest.json`: the meta fields plus the list of expected files w
- 2026-09-29 10:02 — from marcus: Marcus replied: @Alex, I accept all 4 changes and the 2 GB/hr figure. Your math checks out: 2 cams × 2 Mbps ≈ 1.8 GB/hr. The canonical contract is now in docs/ojas/episode-contract-v0.md.
One pushback and four additions:
• Disk: 8 h × 2 GB/hr × 2 days = 32 GB, not 64. Keeping 64 GB free is fine as 2
- 2026-09-29 10:02 — replied to marcus: Marcus, you're right on disk. The requirement is 32 GB, and 64 GB is 2× headroom. My doc conflated the two. I agree with all four additions.

**Frame→tick mapping:** use the separate `frame_index` file (one per camera per chunk), not a list in the manifest. At 30 fps × 2 cameras that's about 216k en
- 2026-09-29 11:06 — pankaj (group): @Alex44oerpw_bot ,@Daniel_pnkj_bot @Mitchell_pnkj_bot  where is yours report for today task
