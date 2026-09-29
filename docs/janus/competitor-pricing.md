# Janus: competitor pricing (INR)

Owner: Mitchell · Task: T-006 · For: Daniel (T-004 icp-pricing-v0 §3, §4a, and T-008 reconciliation) · Captures: 4 Oct · Table: 5 Oct
Status: **SKELETON. No cell filled.** Every cell is UNVERIFIED until it is backed by a capture in `docs/janus/pricing-captures/` with a date.

## Rules
- Nothing is filled from memory. Each cell cites a capture file (or URL) plus a verified-on date.
- INR from an Indian IP only. Record GST as **excl.**, **incl.** or **not stated**, exactly as the page says.
- Contact-sales tiers are marked `CONTACT SALES`. If a limit isn't on the page, the cell says `NOT SHOWN`. No estimates.
- "First tier with automation" means the cheapest tier with workflow rules or automation. Name the vendor's feature.

## Vendors and pricing pages (confirm URL at capture)
| Vendor | Pricing page |
|---|---|
| Zoho CRM | https://www.zoho.com/in/crm/zohocrm-pricing.html |
| Zoho Bigin | https://www.bigin.com/in/pricing.html |
| LeadSquared | https://www.leadsquared.com/pricing/ |
| Freshsales | https://www.freshworks.com/crm/pricing/ |
| Bitrix24 | https://www.bitrix24.in/prices/ |

## 1. Free tiers (PRIORITY, per pankaj, for icp-pricing-v0 §4a)
Verdict against Daniel's §4a belief: **confirmed / contradicted / not shown**.

| Vendor | §4a belief | Free plan exists? | User cap | Record cap | Storage cap | Key features missing vs first paid tier | Verdict | Source, date |
|---|---|---|---|---|---|---|---|---|
| Zoho CRM | Yes | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | – | – |
| Zoho Bigin | Yes, possibly 1 user | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | – | – |
| Freshsales | Yes | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | – | – |
| Bitrix24 | Yes | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | – | – |
| LeadSquared | No (trial/demo only) | UNVERIFIED | n/a | n/a | n/a | n/a | – | – |

For each vendor, check whether the free plan includes: automation / workflows, pipeline or dashboard reports, WhatsApp integration, data import, mobile app, and support channel. Those are the features Janus Free and paid plans are built around.

## 2. Paid tiers
Columns: Tier name · ₹/user/mo (monthly billing) · ₹/user/mo (annual billing, per-month equivalent) · GST · Min users / lock-in · Setup fee · Notes · Source + date

### Zoho CRM
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

### Zoho Bigin
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

### LeadSquared
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | may be CONTACT SALES | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | may be CONTACT SALES | |

### Freshsales
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

### Bitrix24
If Bitrix24 prices per organisation with a user cap, record the flat price and the cap, and show ₹/user at the cap separately. Compare it with Janus's flat team pricing (§3).

| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | pricing unit: | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

## 3. Contradictions with icp-pricing-v0 §4a (filled 4 Oct)
Each line gives the vendor, what §4a says, what the capture shows, and the capture file.
- (none yet)

## Capture checklist (for pankaj → docs/janus/pricing-captures/, 4 Oct)
Capture from an Indian IP, logged out. Name each file like this: `<vendor>-<what>-2026-10-04.png` (or `.pdf`).

For each of the 5 vendors:
1. **Free plan (priority).** A full-page capture of the compare-plans table, ideally via "Save as PDF", showing the free plan's user, record and storage caps and the feature rows. For LeadSquared, capture the pricing page even if it shows no free plan.
2. The pricing page with the **monthly** billing toggle, all tiers visible.
3. The same page with the **annual** billing toggle.
4. Fine print showing GST/tax wording, minimum users, and any setup or onboarding fee.
5. The compare-plans row showing which tier first includes workflow automation. This is usually in the same capture as item 1.

Save the page URL in `docs/janus/pricing-captures/urls.txt`, one line per vendor.

## What I could not verify (as of 2026-09-29)
- Everything in this doc. There was no live web access and no captures yet.
- Whether free-plan limits shown on the Indian page differ from the global page.
