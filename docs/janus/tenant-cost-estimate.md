# Janus — infra cost per small tenant (v0 estimate)

Owner: Marcus · 2026-09-29 · For: Daniel, docs/janus/icp-pricing-v0.md (free-tier cost section)
Status: **ESTIMATE.** These are list prices from memory and must be verified before we quote. The cloud and region are not decided yet (pankaj). Nothing has been built or measured.

## Tenant profile (from Daniel)
- 5 users, 5,000 contacts, 20k activity rows/month, 1 GB of attachments
- Shared multi-tenant Postgres with tenant_id isolation (RLS)

## Size math
| Item | Assumption | Size |
|---|---|---|
| Contacts | 5,000 × ~2 KB (row + indexes) | ~10 MB |
| Activities | 20k/mo × ~1 KB (row + indexes) | ~20 MB/mo → ~250 MB at month 12 |
| Audit log | ~same volume as activities, ~0.5 KB/row | ~10 MB/mo |
| DB total | | ~30 MB (month 1) → ~400 MB (month 12) |
| Attachments | object storage | 1 GB + backup copy |
| Load | 5 users × ~500 req/day | ~2.5k req/day, which is negligible |

## Marginal cost of one extra tenant (per month)
| Line | Basis (list price, verify) | ₹/mo |
|---|---|---|
| Attachments storage | 1 GB × ~$0.02/GB (S3 or R2) + backup copy | 3–5 |
| Egress | ~1–3 GB downloads × ~$0.09/GB (0 on R2) | 0–25 |
| DB storage + backups | ≤0.4 GB × ~$0.12/GB × 2 | 2–10 |
| Transactional email | ~1–3k emails × $0.10/1k | 10–25 |
| Logs/monitoring share | | 5–15 |
| **Total marginal** | | **~₹30–80** |

## Fixed platform (shared by all tenants)
- App: 2× small instances (2 vCPU / 4 GB), ~$60–80
- Managed Postgres: small instance with automated backups, ~$50–90
- Misc: load balancer, DNS, monitoring, CI, ~$15–30
- **Total: ~$125–200/mo ≈ ₹10–17k/mo**
- There is no Redis or Kafka at this stage. Jobs run as a Postgres-backed queue.

## Fully loaded per tenant (fixed ÷ tenants + marginal)
| Tenants | ₹/tenant/mo |
|---|---|
| 50 | ~230–420 |
| 100 | ~130–250 |
| 500 | ~50–110 |

One platform of this size should carry several hundred tenants of this profile before the DB instance needs to grow. That is an estimate, not a load test.

## Excluded (these are likely bigger than infra)
- WhatsApp Business / SMS fees: per-conversation, and likely the largest variable cost if we ship it
- Payment gateway fees, ~2% of revenue
- Any AI/LLM features
- Support time

## Recommendations for free tier
- Hard cap attachments at 1 GB. This is the only line that grows without bound, together with egress.
- Cap or exclude WhatsApp/SMS on the free tier.
- Re-estimate once the region is chosen and prices are verified.
