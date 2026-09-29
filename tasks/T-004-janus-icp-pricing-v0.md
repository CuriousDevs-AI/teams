---
id: T-004
title: Janus ICP + pricing v0
owner: daniel
project: janus
priority: P0
status: doing
assigned: '2026-09-29'
due: '2026-10-01'
depends_on: []
reviewer: pankaj
goal: ''
created_by: pankaj
blocked_on: ''
status_since: '2026-09-29'
updated: '2026-09-29'
rework: true
---

## Goal
Define who Janus is for, who it is not for, and first pricing hypothesis. Basis for interviews and the revenue forecast.

## Done means
- [ ] docs/janus/icp-pricing-v0.md exists
- [ ] One primary ICP segment with size, current tool and switch trigger
- [ ] Explicit 'not for' list
- [ ] 2 price points in INR/month with free-tier cost estimate
- [ ] Open questions to validate in interviews

## Log
- 2026-09-29 — pankaj: doing — changes requested
- 2026-09-29 — daniel: T-007 (segment size) moved to 6 Oct because Mitchell is off until 4 Oct. No impact: §1 already states 'not sourceable, bottom-up'. Any sourced count gets folded in on 6 Oct with the pricing reconciliation.
- 2026-09-29 — daniel: review
- 2026-09-29 — daniel: Shipped v0 (rev 3). Competitor anchors are UNVERIFIED because captures slipped to 4 Oct and T-006 lands 5 Oct. Reconciliation is tracked as a separate task due 6 Oct.
- 2026-09-29 — daniel: §4 filled from Marcus (docs/janus/tenant-cost-estimate.md): infra ₹30–80 marginal, free tenant ≈ ₹340–390/mo including support. §1 segment size set to 'not sourceable, bottom-up'. Only the competitor anchors (T-006, 30 Sep) remain before review.
- 2026-09-29 — daniel: Segment size: Mitchell reports no public dataset cuts trading × 10–100 employees (docs/janus/segment-size-sources.md). §1 will state 'not sourceable, sized bottom-up from the T-005 list'. The done-means 'size' check is met by that stated definition, not by an estimate.
- 2026-09-29 — daniel: doing
- 2026-09-29 — daniel: Draft v0 written: ICP, not-for list, 2 price points, free-tier cost model and open questions. Still missing: competitor anchors (T-006, 30 Sep), the segment count (Mitchell) and the per-tenant infra cost (Marcus). Moves to review 1 Oct.
- 2026-09-29 — daniel: Mitchell confirmed T-006 spec: 5 vendors × 3 tiers, capture checklist for pankaj. Partial table goes out 30 Sep if captures are incomplete, and contact-sales tiers are marked, not estimated.
- 2026-09-29 — daniel: Fallback agreed with Mitchell. If no pricing captures arrive by 30 Sep 12:00, v0 ships 1 Oct with cost-based price points marked UNVERIFIED vs competitors, and competitor anchors move to the T-005 interview questions.
- 2026-09-29 — daniel: doing
- 2026-09-29 — daniel: Started the ICP / not-for / free-tier cost sections. The competitor price anchors depend on pankaj's screenshots (asked for 30 Sep 12:00) and on Mitchell's table (T-006).
- 2026-09-29 — created by pankaj, assigned to daniel

## Feedback
- 2026-09-29 — pankaj: check thier any own one with free tier

## Output
- 2026-09-29 — daniel: docs/janus/icp-pricing-v0.md
