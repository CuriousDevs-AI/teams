# Week-1 plan: DRAFT for pankaj, 30 Sep 10:00

Status: proposal. Nothing below is assigned until pankaj approves.

## Decisions needed first
1. **Goals.** Proposed:
   - G1: a working Ojas demo.
   - G2: Janus paying customers within 6–10 months.
   - G3: grant applications submitted.
2. **Ojas demo target date.** Everything else plans backwards from this.
3. **Ojas lead.** The config says Sofia, the persona says Alex.
4. **Janus customer discovery calls.** These are external, so they need a 🔴 OK.

## Critical path (proposed)
The Ojas demo scope is not written down, so nobody can build against it. Alex has to define it first.

## Proposed P0 per person (week 30 Sep – 4 Oct)
| Person | P0 | Due | Done means |
|---|---|---|---|
| Alex | Ojas demo spec | 2 Oct | Doc covers what the demo shows, which hardware, which model, a pass/fail criterion, and a list of dependencies on Marcus and Sofia |
| Daniel | Janus MVP scope + target customer profile | 3 Oct | Doc lists the MVP features (≤10), who pays, the price hypothesis, and a list of 10 discovery targets |
| Marcus | Shared infra baseline (repos, CI, envs) for Ojas + Janus | 3 Oct | Both repos build in CI, and the setup is written up in docs/ |
| Sofia | Janus MVP UI skeleton, against Daniel's scope | 4 Oct | Screens for the core flow are running locally |
| Mitchell | Grant shortlist | 2 Oct | India and global grants listed with deadlines, eligibility and the amount for each |
| Ethan | Parth requirements on Ojas | 3 Oct | Doc lists what Parth needs from the Ojas runtime, handed off to Alex |
| Nina | Positioning draft | 3 Oct | One-page narrative that grants and the site can reuse |

## Cross-team dependencies to watch
- Sofia depends on Daniel's scope (needed by 3 Oct).
- Ethan hands off to Alex. Their contract must be written down before Alex freezes the spec.
- Sofia is on 3 projects and Marcus is on 2. They are the bottleneck.

## Not doing this week
- Site rebuild. Keep the current site; Nina's copy goes in later.
- Foundation model and fine-tuning work.
- Any Parth hardware purchase (that would be 🔴 money in any case).
- Ojas dashboards / teleop UI. These wait until the demo spec exists.
