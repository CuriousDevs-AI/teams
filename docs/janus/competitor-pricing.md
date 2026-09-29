# Janus: competitor pricing (INR)

Owner: Mitchell · Task: T-006 · For: Daniel (T-004 ICP + pricing v0) · Due: 2026-10-01
Status: **SKELETON. No price filled.** Every cell is UNVERIFIED until it is backed by a capture in `docs/janus/pricing-captures/` or a live page check with a date.

## Rules
- Nothing is filled from memory. Each cell cites a capture file or URL plus a verified-on date.
- INR prices must come from an Indian IP. Global pages may show USD or EUR.
- GST: record whether the displayed price is **excl.** or **incl.** 18% GST, exactly as the page states. If the page doesn't say, write "not stated".
- Contact-sales tiers are marked `CONTACT SALES`. No estimates.
- "First tier with automation" means the cheapest tier that includes workflow rules or automation. Name the feature the vendor uses.

## Vendors and official pricing pages (confirm URL at capture)
| Vendor | Pricing page |
|---|---|
| Zoho CRM | https://www.zoho.com/in/crm/zohocrm-pricing.html |
| Zoho Bigin | https://www.bigin.com/in/pricing.html |
| LeadSquared | https://www.leadsquared.com/pricing/ |
| Freshsales (Freshworks CRM) | https://www.freshworks.com/crm/pricing/ |
| Bitrix24 | https://www.bitrix24.in/prices/ |

## Table
Columns: Tier name · ₹/user/mo (monthly billing) · ₹/user/mo (annual billing, per-month equivalent) · GST · Min users / lock-in · Setup fee · Free tier limits (free row only) · Source + verified-on

### Zoho CRM
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Limits / notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Free | UNVERIFIED | – | – | – | UNVERIFIED | – | UNVERIFIED | – |
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

### Zoho Bigin
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Limits / notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Free | UNVERIFIED | – | – | – | UNVERIFIED | – | UNVERIFIED | – |
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

### LeadSquared
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Limits / notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Free | UNVERIFIED (may not exist) | – | – | – | UNVERIFIED | – | UNVERIFIED | – |
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | may be CONTACT SALES | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | may be CONTACT SALES | |

### Freshsales
| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Limits / notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Free | UNVERIFIED | – | – | – | UNVERIFIED | – | UNVERIFIED | – |
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

### Bitrix24
Note: Bitrix24 may price per organisation with a user cap, not per user. If so, record the flat price and the cap, and compute ₹/user at the cap in a separate note.

| Tier | Name | Monthly | Annual | GST | Min users / lock-in | Setup fee | Limits / notes | Source, date |
|---|---|---|---|---|---|---|---|---|
| Free | UNVERIFIED | – | – | – | UNVERIFIED | – | UNVERIFIED | – |
| Entry paid | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | pricing unit: | |
| First w/ automation | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | UNVERIFIED | automation feature: | |

## Capture checklist (for pankaj → docs/janus/pricing-captures/)
Capture from an Indian IP, logged out, in a normal browser window. Name each file like this: `<vendor>-<monthly|annual>-2026-09-30.png`.

For each of the 5 vendors:
1. Pricing page with the **monthly** billing toggle, all tiers visible.
2. The same page with the **annual** billing toggle.
3. Footnote or fine print showing tax/GST wording, minimum users, and any setup or onboarding fee.
4. The free plan's limits (users, records, storage). This is often on the "compare plans" section.
5. Compare-plans row showing which tier first includes workflow automation.

Save the page URL in `docs/janus/pricing-captures/urls.txt`, one line per vendor.

## What I could not verify (as of 2026-09-29)
- Every price, limit and GST treatment. There was no live web access this session.
- Whether LeadSquared publishes INR list prices at all, or is contact-sales for every tier.
- Whether the Bitrix24 India page is per-org or per-user in 2026.
