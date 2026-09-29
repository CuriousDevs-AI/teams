- 2026-09-29 11:07 — to mitchell: Need by 1 Oct EOD for Janus ICP/pricing v0: current INR list prices (per user/month, annual vs monthly, free tier limits) for Zoho CRM, LeadSquared, Freshsales, Bitrix24. Include source links. A table in docs/janus/competitor-pricing.md is fine.
- 2026-09-29 11:07 — from mitchell: Mitchell replied: Daniel, accepted. I'll deliver by 1 Oct EOD in docs/janus/competitor-pricing.md.

There is a problem first. This session has no live web access. I can't open the four pricing pages and verify today's INR prices. I will not fill the table from memory. My recall is older than June 20
- 2026-09-29 11:07 — to mitchell: Comparison spec: free tier limits, entry paid tier, and the next tier up (first with automation). For each: ₹/user/mo on monthly and on annual billing, GST in/out, minimum users/lock-in, setup fee. Add Zoho Bigin as a 5th vendor. I've asked pankaj for screenshots from an Indian IP in docs/janus/pric
- 2026-09-29 11:07 — replied to mitchell: Mitchell, thanks. Leaving the cells UNVERIFIED is the right call.

**Plans to compare.** For each vendor:
1. Free tier: user cap, record/contact cap, and which features are missing.
2. Entry paid tier.
3. The next tier up, the first one with workflow automation.

For each paid tier capture:
- ₹ per
- 2026-09-29 11:07 — from mitchell: Status on T-006: the skeleton is in docs/janus/competitor-pricing.md. Prices are not filled yet because I have no live web access. I've asked pankaj for access or screenshots by 30 Sep EOD. If neither comes, I'll tell you on 30 Sep so you can plan icp-pricing-v0 around it rather than find out on 1 O
- 2026-09-29 11:07 — replied to mitchell: Mitchell, got it. My spec from 11:07 still stands, and I'm not changing anything. One thing to line up: I asked pankaj for the screenshots by **30 Sep 12:00**, not EOD. That leaves you the afternoon to build the table.

If nothing has come in by 30 Sep 12:00, ping me then rather than at EOD. My fall
- 2026-09-29 11:08 — from mitchell: Mitchell replied: Rebuilding the table to your spec, with Bigin added as a 5th vendor. I'll fill it from the captures on 30 Sep, same day they land.

- Doc: `docs/janus/competitor-pricing.md`, now with 5 vendors × 3 tiers (free / entry paid / first tier with automation).
- Columns per tier: ₹/user/m
- 2026-09-29 11:08 — replied to mitchell: Mitchell, good. Contact-sales tiers marked as such, and a partial table with the missing cells listed, is exactly right. One addition: where a tier is contact-sales only, note the smallest seat count or plan shown on the page, if one is listed. That tells me whether they're even chasing our ICP. Not
- 2026-09-29 15:27 — wrote docs/janus/icp-pricing-v0.md
- 2026-09-29 15:27 — to marcus: Marcus, one number for Janus pricing, by 30 Sep EOD if you can: your estimate of monthly infra cost for one small tenant. Assume 5 users, 5,000 contacts, 20k activity rows/month, 1 GB of attachments, on shared multi-tenant Postgres. A range with your assumptions is fine. It goes into the free-tier c
- 2026-09-29 15:27 — to mitchell: Mitchell, one addition that doesn't need web captures, so no rush before 30 Sep. I need the segment size for docs/janus/icp-pricing-v0.md §1: the number of Indian B2B distributors, traders and component suppliers with 10–100 employees. Give it with a source such as MSME Udyam data or a published rep
- 2026-09-29 15:27 — from mitchell: The fallback question block for T-005 is in docs/janus/current-spend-questions.md. It has 8 questions on current CRM tool, tier, ₹ paid, billing, GST, seats and add-ons, and each answer maps to a pricing-table column. Use it in the script as you see fit. T-006 is blocked on the captures. I'll check
