# Janus: ICP + pricing v0

Owner: Daniel · Task: T-004 · Draft: 2026-09-29 · Status: DRAFT (goes to review 1 Oct)

Every ₹ figure below is **our proposal**, not a market fact. Nothing here gets quoted to a prospect until pankaj approves it and the interviews (T-005) test it.

## 1. Primary ICP (one segment)

**Owner-led Indian B2B distributors, traders and component suppliers.** Profile:
- 10–100 employees, with **3–15 people doing sales or follow-up**.
- Start in NCR, which is where pankaj can reach buyers in person.

| Field | Value |
|---|---|
| Buyer | Owner or MD, who is also the payer. There is no separate procurement. |
| User | Sales reps and the coordinator who handles quotes and follow-ups |
| Current tool | Excel or Google Sheets, WhatsApp (personal numbers), Tally for billing. Occasionally a lapsed Zoho or Bigin trial. *(Hypothesis. Verify in interviews.)* |
| Pain | Leads and quote follow-ups are lost when a rep leaves or goes quiet. The owner cannot see the pipeline without calling each rep. |
| Switch trigger | Headcount crosses about 5 reps, **or** a rep leaves and takes the customer relationships and WhatsApp history with him, **or** a large deal is lost for lack of follow-up. |
| Why us over Zoho/Freshsales | Setup done for them in 1 day (import from their Excel), a WhatsApp-first workflow, flat team pricing, and Hindi/English support on a phone call. *(Positioning hypothesis.)* |
| Segment size | **UNKNOWN.** Mitchell has been asked for a sourced count. I will not estimate it. |

## 2. Not for (v0)

- Solo freelancers and 1–2 person shops. They won't pay, and the support cost equals that of a real customer.
- Companies with 200+ employees. They need SSO, audit, custom roles and procurement cycles.
- Pure B2C and retail with high-volume leads (edtech, real estate call centres). That is LeadSquared's home turf, and it needs telephony and a dialer.
- Anyone who needs an ERP, inventory or GST invoicing on day one. We integrate with Tally later and do not replace it.
- Companies outside India. The pricing, support hours and language are India-only for v0.
- Buyers who want deep customisation before they pay. That is the services trap.

## 3. Price points (proposed, GST 18% extra)

Flat team pricing, not per seat. The hypothesis is that owners in this segment resist per-user billing. Interviews will test that.

| Plan | Users | Monthly billing | Annual billing (per month) | Includes |
|---|---|---|---|---|
| **Team** | up to 5 | ₹2,499 | ₹1,999 | Contacts, deals pipeline, follow-up reminders, Excel import, WhatsApp click-to-chat logging, owner dashboard |
| **Business** | up to 15 | ₹5,999 | ₹4,999 | Everything in Team, plus simple automation (auto-assign, overdue alerts), rep-wise reports, 1 onboarding call plus data import done by us |

- Extra user beyond 15: ₹399/mo, a placeholder to be decided later.
- No setup fee in v0. Removing friction matters more than setup revenue for the first 10 customers.
- **Competitor anchors: PENDING.** They come from T-006 (Mitchell) once pankaj's captures land on 30 Sep. If our Team price comes out above the entry per-seat price of Zoho Bigin or Zoho CRM for 5 users, I will revise it here before review.

## 4. Free tier: cost and recommendation

**Recommendation: no free tier at launch.** Use a 14-day trial with a done-for-you import instead.

Cost model for one free tenant per month. All inputs are assumptions until measured:

| Item | Assumption | ₹/tenant/mo |
|---|---|---|
| Infra (shared Postgres, storage, email) | Marcus asked for an estimate | UNKNOWN |
| Support | 2 tickets × 15 min, plus a one-off 45-min onboarding call amortised over 6 months ≈ 37 min/mo | ≈ ₹310 at an assumed ₹500/hr loaded cost |
| **Total** | | **≈ ₹310 + infra** |

Why no free tier:
- For this segment, support cost dominates infra cost.
- A free user costs about as much to support as a paying one.
- At 100 free tenants that is roughly ₹31k/mo plus infra, and it buys no revenue signal.
- We need 10 paying customers, not 100 free ones.

Revisit a free tier only after activation is measured on paid trials.

## 5. First-10-customer maths (planning input, not a forecast)

- 10 customers at a 60/40 Team/Business mix on annual billing: 6 × ₹1,999 + 4 × ₹4,999 = **₹31,990/mo MRR** (≈ ₹3.84 L ARR), excluding GST.
- This is **not** a forecast. The conversion rates and sales cycle are unknown until interviews and a first pilot. Ojas and Parth should not plan on this number yet.

## 6. Open questions for interviews (T-005)

1. What do they use today to track leads and follow-ups, and who maintains it?
2. What do they pay today for software per month, in total and per tool? *(This is our real price anchor.)*
3. Have they tried Zoho, Bigin, Freshsales or LeadSquared? Why did they stop?
4. Flat team price or per user: which do they prefer, and why?
5. Would they pay ₹1,999–₹2,499/mo for up to 5 users? If not, what number?
6. Is WhatsApp logging a must-have or a nice-to-have? How many of their deals start on WhatsApp?
7. Monthly or annual billing? Would a 2-months-free annual offer move them?
8. Who decides, and how long does it take from "yes" to paying?
9. What would make them churn in month 2?
10. Would they need Tally sync to pay at all, or can it come later?

## 7. What's missing before review

- [ ] Competitor anchors in §3 (T-006, 30 Sep)
- [ ] Segment count in §1 (Mitchell)
- [ ] Infra cost in §4 (Marcus)
