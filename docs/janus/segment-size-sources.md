# Janus: segment size sources (Indian B2B distributors, traders, component suppliers, 10–100 employees)

Owner: Mitchell · For: Daniel, docs/janus/icp-pricing-v0.md §1 · Written: 2026-09-29
Status: **No number.** No live web access this session. Everything below is from memory and marked TO VERIFY. No estimates.

## The core problem
Indian official MSME data classifies firms by **investment and turnover** (micro, small, medium), not by **employee count**. So a direct count of "trading firms with 10–100 employees" may not exist in any public dataset. This needs to be confirmed or ruled out with live access.

## Candidate sources (all TO VERIFY)
| Source | What it might give | Likely gap |
|---|---|---|
| Udyam Registration portal / dashboard (Ministry of MSME) | Registered enterprise counts by activity (manufacturing, services, trading) and by micro/small/medium class | Size class is by turnover/investment, not headcount. Employment may be self-reported and only in aggregate. Registration is voluntary. |
| ASUSE (Annual Survey of Unincorporated Sector Enterprises, MoSPI) | Enterprise counts by activity. Possibly a split by hired-worker establishments or worker size. | Unincorporated firms only, so companies and LLPs are excluded. Check whether worker size classes are published. |
| Economic Census (MoSPI) | Establishment counts by activity and employment size | Latest full results may be old or unreleased. Check availability. |
| EPFO establishment registrations | Count of establishments with 20+ employees (mandatory registration) | Not by activity and not a 10–100 band. A proxy at best. |
| MCA company master data | Incorporated companies filterable by NIC code for wholesale trade | No employee counts. |
| Industry reports (e.g. distribution-sector studies) | A possible market-size figure | Methodology is often opaque. Use only if the method is stated. |

## Output rule for §1
One of:
1. A count with source URL, dataset, year, and the exact definition used, or
2. "Not sourceable from public data as of <date>". If a proxy is used, name it and state its definition.

## Needs
Live web access to check each source above (asked pankaj, 2026-09-29).
