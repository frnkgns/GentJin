---
name: cleanup
description: Finish and QA current implementation work before handoff, review, or merge. Use when the user asks to clean up, and also when work is being wrapped up or judged production-ready. Reviews the Git diff, runs the project's real tests/typecheck/lint/build, removes temporary and dead code, checks data/query/frontend/security quality, updates knowledge, and always finishes with a cleanup report.
---
# Cleanup

## Scope
Treat `clean up` as finishing current work. Inspect staged/unstaged changes first and focus on changed files plus directly related code needed for correctness. Do not refactor unrelated code.

If no pending Git changes exist, state that and still perform relevant knowledge/WIP/inbox consistency checks. If not in Git, state Git checks were skipped.

## Review and Fix
- Understand the diff before editing.
- QA the affected user flow and edge/error/loading/empty states.
- Fix in-scope problems rather than only reporting them.
- If a defect appears, load and follow `systematic-debugging` before changing code.
- Use `test-strategy` to decide what the change needs verified, then run it.
- Remove unnecessary `console.log`, temporary debugging, dead code, unused imports/variables/functions/components/hooks, duplicate logic, redundant conditions/state/queries, obsolete comments, and stale commented-out code.
- Keep `console.error` for genuine errors; use `console.warn` only when appropriate; preserve intentional structured logging.
- Remove mock/placeholder/hardcoded business data that should use real application data.

## Verification
Discover actual project commands from `.project-agent.md`, package metadata, or equivalent. Run applicable typecheck, lint, test, and build commands. Never claim a check passed unless it ran. Do not suppress errors with `any`, `@ts-ignore`, or disabled lint rules without a documented technical reason.

## Conditional Reviews
When relevant, invoke/apply:
- `frontend-review` for changed UI/UX.
- `backend-review` for handlers, services, jobs, webhooks, or external integrations.
- `database-review` for queries, schema, or data work.
- `security-review` for auth, permissions, sensitive data, mutations, or external input.
- `integration-review` when the change crosses a layer boundary.
- `api-design` for contracts, pagination, versioning, or error shapes.
- `architecture-review` for cross-module or structural risk.
- `refactoring-migration` when the diff is part of a staged migration or flag lifecycle.
- `test-strategy` when verification is missing or insufficient.
- `systematic-debugging` when a finding needs a root cause.
- `performance-review` when cost is measurable and suspicious.
- `dependency-review` when dependencies change.
- `deployment-readiness` when the change is about to ship or touches deploy or migration safety.
- `incident-response` when the finding is live production impact.

For a full read-only analysis of the changes, use `/changes-review`. Cleanup finishes the work the user is completing; it is not a second review workflow.

## Final Validation
Re-check the diff. Confirm temporary artifacts/logs are gone, real data is used, applicable checks pass, changed queries/screens/failure states were reviewed, and no obvious regression remains. Explicitly report anything that could not be verified.

## Knowledge and Report
Triage inbox, archive answered questions/completed WIP when appropriate, and keep reusable knowledge synchronized. Cleanup always ends with the `reporting` workflow: save a detailed cleanup report under `reports/` and print the concise console report.
