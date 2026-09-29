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
- 2026-09-29 15:27 — replied to mitchell: Mitchell, thanks. I'll pull `docs/janus/current-spend-questions.md` into the T-005 script by 2 Oct. The script is capped at 10 questions, so your 8 go in as a fill-in sheet the buyer completes after the call, and the script keeps 2 of them: current tool and ₹/month paid. Those answers become our rea
- 2026-09-29 15:27 — from mitchell: Mitchell replied: I can't give you a sourced number today. It isn't only the missing web access: I don't know of any official Indian dataset that counts wholesale and trading firms by a 10–100 employee band. Udyam classifies firms as micro, small or medium by investment and turnover, not by headcoun
- 2026-09-29 15:27 — replied to mitchell: Mitchell, agreed, and that's the honest answer. For v0 I don't need a national TAM. The first-10-customers plan depends on reachable buyers, not on market size. §1 will read: **"Not sourceable from public data as of 2026-10. Sizing is bottom-up from the T-005 target list."** Please timebox T-007 to
- 2026-09-29 15:27 — from marcus: Marcus replied: Daniel, the estimate is in docs/janus/tenant-cost-estimate.md. Paste whichever line fits §free-tier.

Per small tenant:
- Marginal cost (what one more free tenant adds): ₹30–80/month. Attachments, storage, backups, egress and email only. Compute is shared.
- Fully loaded (tenant's sh
- 2026-09-29 15:28 — wrote docs/janus/icp-pricing-v0.md
- 2026-09-29 15:28 — replied to marcus: Marcus, thanks, that's what I needed. The marginal ₹30–80 goes into §4. Support at about ₹310 is still the bigger line, so a free tenant costs about ₹340–390/mo and 'no free tier at launch' stands. On WhatsApp, v0 is click-to-chat logging only, meaning wa.me links with no API, so there are no per-me
- 2026-09-29 15:28 — from mitchell: Daniel, heads-up ahead of our 30 Sep 12:00 checkpoint: Pankaj will provide the pricing captures on 4 Oct, so the competitor table (T-006) lands 5 Oct, not 30 Sep. Please ship T-004 with competitor prices marked UNVERIFIED as agreed. docs/janus/current-spend-questions.md is final for T-005, and the b
- 2026-09-29 15:28 — wrote docs/janus/icp-pricing-v0.md
- 2026-09-29 15:28 — replied to mitchell: Mitchell, noted: captures 4 Oct, table 5 Oct. T-004 goes to review now with the anchors marked UNVERIFIED. I've added a 6 Oct task to reconcile our prices against your table. Interview price questions (§6) stand as they are, since buyer spend is the better anchor anyway.
- 2026-09-29 15:29 — from mitchell: Daniel, one more change: Pankaj has put me off until 4 Oct, so T-007 (segment size) moves from 2 Oct to 6 Oct. The answer will still be either a sourced count or a written 'not sourceable' with the proxies labelled, same as I told you earlier. If your ICP doc needs something sooner, write 'segment s
- 2026-09-29 15:29 — replied to mitchell: Mitchell, noted, 6 Oct is fine. Nothing in v0 is waiting on it: §1 already says not sourceable, sized bottom-up from the T-005 list, and cites T-007. I'll fold in your result with the 6 Oct pricing reconciliation. If you find a sourced count, send it with the definition used. If not, no need to send
- 2026-09-29 15:36 — wrote docs/janus/icp-pricing-v0.md
- 2026-09-29 15:36 — to mitchell: Mitchell, a priority on the 4 Oct captures: pankaj wants competitor free tiers checked. For each of the 5 vendors, the free-tier row needs to be complete: whether a free plan exists, the user cap, the record cap, and the key features missing. docs/janus/icp-pricing-v0.md §4 lists what I believe from
