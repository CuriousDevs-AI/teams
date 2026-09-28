# Work style & culture — proposal from Alex (Ojas)

Date: 2026-09-29. This is written for our stage: pre-revenue, small team, first demo, grants.

## 1. Evidence over opinion
- Every status update names an artifact: a commit, a file in `docs/`, a measured number, or a log.
- Every claim has a number and the conditions behind it. Write "p99 42 ms at 30 Hz on Orin NX, 200 trials", not "fast".
- Say "I don't know" early. Guesses must be labelled as guesses.

## 2. Contracts before code
- Any interface between two people is written down before either side builds. That includes APIs, schemas, file formats and copy structure.
- Examples: Ojas↔Parth runtime API, Ojas↔Marcus device/cloud boundary, Janus front/back.
- The contract lives in `docs/<project>/`. Changing a shared contract is 🟡: do it, then tell pankaj and the affected owner.

## 3. Safety, evals, observability are not optional
- Nothing acts on hardware from an unchecked model output. The safety envelope and fallback exist from day one.
- Skipping a safety, eval or telemetry step is allowed only as a written decision on the task. Write who decided and until when.
- For robots: e-stop is tested before every session, and no unattended runs.

## 4. One real demo a week
- Every Friday, each project shows something running: on hardware, in a terminal, or deployed. Slides and videos don't count.
- The demo result gets logged on the relevant task, including failures.

## 5. Async by default
- The board (`tasks/`) is the only status source. James reads it, so nobody chases people.
- HQ is for announcements only. Teammates talk through the inbox.
- Meetings happen only to unblock someone or to make a decision. Each ends with a written outcome on a task.
- Blocked means you name the person and the exact ask, within the same day.

## 6. Small, finished pieces
- Tasks are ≤5 days, with at most 2 in `doing` (already in the charter). Prefer merging something small and measured over a big "almost done" branch.
- Every design starts with the smallest measurable version, then states the upgrade path.

## 7. Direct and blameless
- Disagree in writing with a reason and an alternative. pankaj's call is final once made.
- When a robot, a deploy or data surprises us, write a short incident note within 24h: what happened, why, and what changes. No blame.
- Corrections from pankaj are saved and never repeated.

## 8. Open gap
- The charter's Goals section is empty. Every task has to serve a goal, so we should set 2–4 goals first. A possible example: "first working Ojas+Parth demo" and "grant applications submitted".
