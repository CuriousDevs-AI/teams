# Janus: "What do you pay today" question block (for T-005 interviews)

Owner: Mitchell · Supports: T-006 fallback, T-005 script (Daniel) · Written: 2026-09-29
Purpose: get **actual spend** from buyers. This is stronger evidence than list prices. Each answer maps to a column in `docs/janus/competitor-pricing.md`, so interview data and list prices sit in one table.

## Questions (about 5 min, near the end of the interview)
1. **Tool:** What do you use today to track leads and customers? (Name the tool. Spreadsheet, WhatsApp and "nothing" all count.) → *Vendor*
2. **Plan:** Which plan or edition are you on? Could you show me the billing page? → *Tier name*
3. **Seats:** How many paid users or seats do you have? How many people actually log in each week? → *Min users / real usage*
4. **Price:** What do you pay per month in total? Is that per user or a flat fee? → *₹/user/mo*
5. **Billing:** Do you pay monthly or annually? Did you get a discount for paying annually? → *Monthly vs annual*
6. **Tax:** Does that figure include GST? Do you claim input credit on it? → *GST in/out*
7. **Extras:** Did you pay for setup, onboarding, a partner, add-ons (WhatsApp, telephony, SMS) or extra storage? → *Setup fee / add-ons*
8. **Switch:** What would make you switch? At what monthly price would you stop considering switching? → *Anchor / willingness to pay*

## Recording rules
- Write the number exactly as the buyer said it and note whether it came from memory or from a bill or screen shown. Answers taken from an invoice or screen count as verified.
- Don't prompt with competitor prices. That anchors the buyer.
- Log each interview as a row: date, company size, tool, tier, seats, ₹/mo total, billing, GST, extras, source (memory or bill).

## Log template
| Date | Company (size) | Tool | Tier | Seats (paid/active) | ₹/mo total | Billing | GST | Extras | Source |
|---|---|---|---|---|---|---|---|---|---|
