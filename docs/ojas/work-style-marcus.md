# Work style — backend & infra additions (Marcus)

Builds on Alex's proposal (docs/ojas/work-style-proposal.md). I agree with all four of Alex's points. This doc only adds to them.

## 1. Contracts change through PRs
- Any change to a shared API, event schema or episode format goes through a PR that shows the contract diff (types/JSON schema/OpenAPI).
- Someone from the consuming side (Sofia, Alex or Ethan) approves it before merge.
- Breaking changes need a version bump or a deprecation window. No silent edits.

## 2. Every production change has a rollback
- The PR description states how to undo the change.
- DB migrations follow expand → backfill → contract, and are never destructive in a single deploy.
- Production changes remain 🔴 per the charter: ask pankaj first.

## 3. Cost is stated up front
- A new component (DB, queue, bucket, SaaS tool) must state ₹/month at today's volume, the estimate at 10× volume, and who operates it.
- Nothing is added without that line. New vendors remain 🔴.

## 4. Security is not "for now"
- Auth, tenant isolation, PII and secrets are designed in from the first PR.
- Any shortcut is logged as a decision (same rule Alex has for safety/evals).
- Secrets never go in the repo or in chat.

## 5. Backups are real only after a restore
- We run a restore test on the 1st of every month and log the result on the board with duration and data checked.

## 6. Incidents
- Within 48h, write a short blameless postmortem in docs/ covering what happened, impact, root cause and one fix.
- The fix becomes a task on the board.

## 7. Blockers
- If you're blocked for more than half a working day, say so the same day.
- Set the task to `blocked` and name the person and the exact ask.

## 8. Boring tech by default
- Our defaults are Postgres, one repo, one deploy path and managed services.
- Kafka, Kubernetes or microservices need a measured need first, such as throughput numbers or an ops pain we have actually hit.
