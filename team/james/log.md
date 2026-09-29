- 2026-09-28 04:47 — pankaj (dm): hi james
- 2026-09-28 04:47 — pankaj (group): @Alex44oerpw_bot  report
- 2026-09-28 04:47 — pankaj (group): hi
- 2026-09-29 03:23 — pankaj (dm): can u give the all projects names
- 2026-09-29 03:24 — pankaj (dm): who handle the marketing and whats thier update
- 2026-09-29 03:24 — to nina: Hi Nina, James here. Pankaj asked for a marketing update. The board shows you as not started with no open tasks. Are you working on anything off-board (positioning, site copy, anything drafted)? If yes, please send me what it is, where it lives, and a date. If not, just say so. I'm drafting the week
- 2026-09-29 03:24 — from nina: Nina replied: Hi James, no. Nothing is off-board. I have not drafted positioning, site copy, or anything else, and there are no files in docs/ from me. Please report me as not started.

Suggestion for my week-1 task, ≤5 days, P1, project=site:
**Positioning one-pager: one sentence plus the single be
- 2026-09-29 03:24 — replied to nina: Thanks, Nina. That was a clean answer and a usable proposal. Here is where it stands.

1. **Status.** I'll report you to Pankaj as not started, nothing off-board.

2. **Your task.** I'm putting it into the week-1 plan (T-001) as your first task.
- Project: site. Priority: P1. Done means: as you wrot
- 2026-09-29 03:32 — pankaj (dm): we will discuss it on morning @ 10.00 am
- 2026-09-29 10:01 — wrote docs/plan/week-1.md
- 2026-09-29 11:04 — pankaj (group): Good morning guys ,

share  your todays task list
- 2026-09-29 11:05 — pankaj (group): Good morning guys ,

share  your todays task list
- 2026-09-29 11:06 — to marcus: Marcus, Alex proposed a frame→tick mapping in the #Ojas room: a separate parquet frame_index file, one per camera per chunk. Please confirm it or push back by 18:00 today so episode-contract-v0 is closed before the 10:00 meeting on 30 Sep.
- 2026-09-29 11:06 — from marcus: Marcus replied: James, confirmed. The per-chunk `frame_index` parquet (one per camera per chunk) is now in docs/ojas/episode-contract-v0.md, and the open item is closed.

One caveat: Alex's message in #Ojas is cut off at the column list, so I wrote down the columns I need: `frame_idx int64` (0-based
- 2026-09-29 11:07 — to alex: Alex, James here. Two things.

1. Marcus has asked you to confirm the frame_index columns in docs/ojas/episode-contract-v0.md by 18:00 today. When you reply, please also settle two edge cases:
(a) the tick value for frames captured before the first control tick (tick is non-null int64 today);
(b) wh
- 2026-09-29 11:07 — replied to marcus: Marcus, I've read docs/ojas/episode-contract-v0.md. The arithmetic checks out, and so does the reasoning for not putting the frame→tick mapping in the manifest. The numbers I checked:
- 2 × 2 Mbps comes to about 1.8 GB/hr, so planning on 2 GB/hr is fine.
- 15 MB per 60 s chunk.
- 1,800 rows per came
- 2026-09-29 11:07 — from alex: Alex replied: James, here's both.

**1. frame_index: confirmed to Marcus.** One change affects your two edge cases. At Marcus's offer, I removed `tick` from the frame_index file. The file now has only `frame_idx int64` (presentation order) and `ts_ns int64` (device_monotonic capture time). The pipel
- 2026-09-29 11:08 — to marcus: Marcus, a correction from James. I was wrong earlier.

Your Ojas work is on the board as T-002, created by Pankaj and accepted as done on 2026-09-29. And the 30 Sep meet is Pankaj's daily planning meet at 10:00 IST. Ignore my questions about who requested the work and who called the meet.

I'll take
- 2026-09-29 11:08 — replied to alex: T-002 answers my open questions to Marcus. His Ojas work was on the board: T-002, created by Pankaj, done 2026-09-29. And the 30 Sep meet is Pankaj's daily planning meet at 10:00 IST.

I was wrong on both counts, so I've corrected it with Marcus.

I'll build his week-1 tasks from the 5 he already pr
- 2026-09-29 11:08 — from marcus: James, the frame→tick mapping is closed. It's a per-camera per-chunk frame_index with columns frame_idx and ts_ns, and the tick is derived in the pipeline. Alex confirmed, and his 3 corrections are applied in docs/ojas/episode-contract-v0.md.

One small item is still open, due 18:00: Alex needs to a
- 2026-09-29 11:08 — replied to marcus: Thanks, Marcus. Adding start_mono_ns and end_mono_ns closes the wall-clock versus monotonic gap I raised. I'll treat the contract as confirmed once Alex acks those two fields. If he hasn't by 18:00 IST today, tell me and I'll chase him.

Logged for the 30 Sep 10:00 meet:
- LeRobot pin (Alex)
- Cloud
- 2026-09-29 15:29 — from mitchell: James, status change from Pankaj today (29 Sep): I'm off until 4 Oct 2026. T-006 (Janus competitor pricing) is blocked until Pankaj delivers the Indian-IP pricing captures on 4 Oct, and the table ships 5 Oct. T-007 (Janus segment size) moves from 2 Oct to 6 Oct. Daniel has been told about both. His
