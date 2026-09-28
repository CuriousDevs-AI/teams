# Work style — frontend additions (Sofia)

I agree with Alex's four points (docs/ojas/work-style-proposal.md) and with Marcus's rule that shared schema and API changes go through a contract-diff PR (docs/ojas/work-style-marcus.md). This file only covers what they don't.

## 1. States are part of the feature
A screen needs a loading state, an empty state and an error state before it goes to review. Demos usually fail on exactly these three cases, so they are part of the feature, not polish.

## 2. Mock data is labelled on screen
Any screen showing mock or replayed data displays a visible `MOCK` badge (or `REPLAY` for replayed data). We never present mock data as real, whether in the Friday demo, a grant video or a screenshot.

## 3. Contracts are files, not descriptions
For every endpoint the UI consumes, the repo holds:
- TypeScript types for the request and response, including pagination and the error shape.
- One sample response (JSON) that both backend tests and frontend mocks use.

Marcus's contract-diff PRs need approval from the consuming side. For Ojas UI, that approver is me.

## 4. Performance budget on every UI PR
Each UI PR description states:
- the first-load target and the measured value;
- behaviour at 10,000 rows (virtualised or paginated);
- for live telemetry, whether it holds 30 fps without dropping frames.

Numbers only. "Feels fast" doesn't count.

## 5. Accessibility before review
Checked before review: keyboard navigation, visible focus, labelled inputs and WCAG AA contrast. This takes about ten minutes per screen. Retrofitting later takes days.

## 6. The Friday demo runs on a production build
- Use a production build against the real backend, not a dev server with warm local state.
- Say out loud what is mocked, skipped or known to be broken, before someone finds it.
- Every UI PR includes a screenshot or a short recording.

## Open item
Once agreed, Alex, Marcus's and this doc should be merged into one section of the team charter. I suggest Alex owns the merge.
