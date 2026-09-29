# Janus: ICP + pricing v0

Owner: Daniel · Task: T-004 · 2026-09-29 (rev 4, reworked per pankaj: competitor free tiers) · Status: IN REVIEW. Competitor facts are UNVERIFIED until the 4–5 Oct captures.

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
| Segment size | **Not sourceable from public data as of 2026-10.** Udyam classifies by investment and turnover, not headcount (see docs/janus/segment-size-sources.md, T-007, due 6 Oct). Sizing is bottom-up from the reachable T-005 target list. |

## 2. Not for (v0)

- Solo freelancers and 1–2 person shops as paying customers. They can use Free, but we don't sell to them or support them by phone.
- Companies with 200+ employees. They need SSO, audit, custom roles and procurement cycles.
- Pure B2C and retail with high-volume leads (edtech, real estate call centres). That is LeadSquared's home turf, and it needs telephony and a dialer.
- Anyone who needs an ERP, inventory or GST invoicing on day one. We integrate with Tally later and do not replace it.
- Companies outside India. The pricing, support hours and language are India-only for v0.
- Buyers who want deep customisation before they pay. That is the services trap.

## 3. Plans and price points (proposed, GST 18% extra)

Flat team pricing, not per seat. The hypothesis is that owners in this segment resist per-user billing. Interviews will test that.

| Plan | Users | Monthly billing | Annual billing (per month) | Includes |
|---|---|---|---|---|
| **Free** | up to 2 | ₹0 | ₹0 | Contacts (max 500), deals pipeline, follow-up reminders, Excel import (self-serve), WhatsApp click-to-chat logging. 200 MB attachments. Email support only, 2 business-day reply. No automation, no owner dashboard. |
| **Team** | up to 5 | ₹2,499 | ₹1,999 | Free features without the caps, plus the owner dashboard and phone support. 1 GB attachments. |
| **Business** | up to 15 | ₹5,999 | ₹4,999 | Everything in Team, plus simple automation (auto-assign, overdue alerts), rep-wise reports, 1 onboarding call plus data import done by us. 5 GB attachments. |

- The 14-day Team trial stays for anyone who signs up to buy. Free is for people who aren't ready to buy yet.
- Upgrade triggers:
  - Free → Team: a 3rd user, or contact 501, or wanting to see the whole pipeline (owner dashboard).
  - Team → Business: a 6th user, or wanting automation.
  - These are designed to hit exactly when our ICP's switch trigger happens (the team grows past a few reps).
- Extra user beyond 15: ₹399/mo, a placeholder to be decided later.
- No setup fee in v0.
- Payment gateway fees are about 2% of revenue, roughly ₹40–120 per paying customer per month.
- **Competitor price anchors: UNVERIFIED.** The table (T-006) is due 5 Oct, and reconciliation is due 6 Oct. Until then these prices are not used in outreach.

## 4. Free tier: competitors, cost and recommendation

### 4a. Competitor free tiers (UNVERIFIED, from memory, confirm with the 4–5 Oct captures)

| Vendor | Free plan? (believed) | User / record caps | What's missing on free |
|---|---|---|---|
| Zoho CRM | Yes, believed | TBD from capture | TBD |
| Zoho Bigin | Believed yes, possibly limited to 1 user | TBD | TBD |
| Freshsales | Yes, believed | TBD | TBD |
| Bitrix24 | Yes, believed | TBD | TBD |
| LeadSquared | Believed no (trial / demo only) | n/a | n/a |

**Implication, if confirmed:**
- 3–4 of 5 competitors give a basic CRM away free.
- A prospect who searches "free CRM" will find Zoho, Freshsales or Bitrix24 before us. With no free option, we lose them at the top of the funnel.
- Free therefore cannot be our differentiator. Our paid pitch has to be done-for-you setup, a WhatsApp-first workflow, the owner's pipeline view and phone support. Those are exactly what our Free plan leaves out.

### 4b. Cost of one Free tenant per month

| Item | Source / assumption | ₹/tenant/mo |
|---|---|---|
| Infra, marginal (storage, backups, egress, email; compute is shared) | Marcus, docs/janus/tenant-cost-estimate.md. Cloud list prices not yet verified, region undecided. Our 200 MB cap is below his 1 GB assumption, so this is conservative. | ₹30–80 |
| Support | Self-serve only: about 1 email ticket × 15 min/mo, at an assumed ₹500/hr loaded cost. No onboarding call. | ≈ ₹125 |
| **Total** | | **≈ ₹155–205** |

For comparison, the full-support free tenant from rev 3 cost about ₹340–390/mo. Self-serve with email support only roughly halves that.

### 4c. Recommendation

**Offer a capped, self-serve Free plan, not "no free tier".** The guardrails:
- **Hard cap of 100 Free tenants** until we have activation data. Worst case that costs about ₹15.5–20.5k/mo. After the cap, new signups go on a waitlist or the Team trial.
- No phone support and no done-for-you import on Free. Support cost is the thing that kills Indian SMB SaaS.
- Metrics that decide whether Free stays:
  - % of Free tenants active in week 4
  - Free → paid conversion by day 60
  - support minutes per Free tenant
- Kill or tighten Free if **< 5% convert by day 60** or support exceeds **30 min per tenant per month**. These thresholds are my proposal, not benchmarks.

If the 4–5 Oct captures show competitors do **not** offer usable free tiers, I'll revert to trial-only and say so on 6 Oct.

## 5. First-10-customer maths (planning input, not a forecast)

- 10 customers at a 60/40 Team/Business mix on annual billing: 6 × ₹1,999 + 4 × ₹4,999 = **₹31,990/mo MRR** (≈ ₹3.84 L ARR), excluding GST.
- Offset: Free cost of up to ≈ ₹20.5k/mo at the 100-tenant cap. So at 10 paying customers with a full Free cap, Janus is barely ahead on these two lines alone. This is why the Free cap and kill thresholds matter.
- This is **not** a forecast. The conversion rates and sales cycle are unknown until interviews and a first pilot. Ojas and Parth should not plan on this number yet.

## 6. Open questions for interviews (T-005)

1. What do they use today to track leads and follow-ups, and who maintains it?
2. What do they pay today for software per month, in total and per tool? *(This is our real price anchor. Detail goes in Mitchell's docs/janus/current-spend-questions.md sheet.)*
3. Have they tried Zoho, Bigin, Freshsales or LeadSquared, on a free or paid plan? Why did they stop?
4. Would a free plan make them try us, or do they want someone to set it up for them?
5. Flat team price or per user: which do they prefer, and why?
6. Would they pay ₹1,999–₹2,499/mo for up to 5 users? If not, what number?
7. Is WhatsApp logging a must-have or a nice-to-have? How many of their deals start on WhatsApp?
8. Monthly or annual billing? Would a 2-months-free annual offer move them?
9. Who decides, and how long does it take from "yes" to paying?
10. What would make them churn in month 2, and would Tally sync be needed before they pay?

## 7. Status of inputs

- [ ] Competitor free tiers (§4a) and price anchors (§3): UNVERIFIED, captures 4 Oct, table 5 Oct, reconciliation 6 Oct
- [x] Segment size in §1: not sourceable, stated as such (T-007, due 6 Oct)
- [x] Infra cost in §4b (Marcus, 29 Sep)
