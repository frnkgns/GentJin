---
name: cleanup
description: Perform the user's full pre-main cleanup workflow. Use when the user says clean up/cleanup or asks for production-readiness QA of current changes. Reviews Git changes, debugs issues, runs tests/typecheck/lint/build when available, removes temporary/dead code, checks data/query/frontend/security quality, updates knowledge, and always finishes with a cleanup report.
---
# Cleanup

## Scope
Treat `clean up` as finishing current work. Inspect staged/unstaged changes first and focus on changed files plus directly related code needed for correctness. Do not refactor unrelated code.

If no pending Git changes exist, state that and still perform relevant knowledge/WIP/inbox consistency checks. If not in Git, state Git checks were skipped.

## Review and Fix
- Understand the diff before editing.
- QA the affected user flow and edge/error/loading/empty states.
- Debug issues found; fix in-scope problems rather than only reporting them.
- Run relevant tests and add/improve tests when reasonable.
- Remove unnecessary `console.log`, temporary debugging, dead code, unused imports/variables/functions/components/hooks, duplicate logic, redundant conditions/state/queries, obsolete comments, and stale commented-out code.
- Keep `console.error` for genuine errors; use `console.warn` only when appropriate; preserve intentional structured logging.
- Remove mock/placeholder/hardcoded business data that should use real application data.

## Verification
Discover actual project commands from `.project-agent.md`, package metadata, or equivalent. Run applicable typecheck, lint, test, and build commands. Never claim a check passed unless it ran. Do not suppress errors with `any`, `@ts-ignore`, or disabled lint rules without a documented technical reason.

## Conditional Reviews
When relevant, invoke/apply:
- `frontend-review` for changed UI/UX.
- `database-review` for queries, APIs, server functions, schema/data work.
- `security-review` for auth, permissions, sensitive data, mutations, or external input.

## Final Validation
Re-check the diff. Confirm temporary artifacts/logs are gone, real data is used, applicable checks pass, changed queries/screens/failure states were reviewed, and no obvious regression remains. Explicitly report anything that could not be verified.

## Knowledge and Report
Triage inbox, archive answered questions/completed WIP when appropriate, and keep reusable knowledge synchronized. Cleanup always ends with the `reporting` workflow: save a detailed cleanup report under `reports/` and print the concise console report.
