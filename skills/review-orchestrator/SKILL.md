---
name: review-orchestrator
description: Shared comprehensive review engine for the review commands. Use when `/changes-review` or `/project-review` is invoked, or when the user explicitly asks for a full multi-layer or whole-codebase review, to map scope, delegate to the relevant specialized skills, and produce one evidence-based report. Not for single-file or single-layer review; use the specific review skill directly.
---
# Review Orchestrator

## Purpose

Provide one shared review engine so `/changes-review` and `/project-review` stay consistent and the specialized skills stay small. Coordinate skills; do not duplicate their instructions.

## Detect the scope

- For current work: staged, unstaged, and relevant untracked source files, plus branch changes needed to understand the work.
- For the whole project: the repository map, architecture, and major modules, excluding generated and vendor artifacts.
- Detect the stack first: language, framework, package manager, data layer, auth, deployment target, and available verification commands.

## Determine the impact radius

Do not review only changed lines. When a changed file depends on or affects another component, inspect enough surrounding code to verify the integration: shared types, callers, consumers, persistence, authorization rules, and tests.

## Delegate to the relevant skills

Load only the skills the detected code actually needs:

- `frontend-review` for UI, responsive behavior, accessibility, and frontend state.
- `backend-review` for handlers, services, jobs, webhooks, and async behavior.
- `database-review` for queries, schema, data integrity, and migration safety.
- `security-review` for authentication, authorization, secrets, input, and sensitive logging.
- `integration-review` for the seams between layers.
- `api-design` for contracts, pagination, versioning, and error shapes.
- `architecture-review` for cross-module or structural risk in the change.
- `refactoring-migration` when the change is a staged migration, expand-contract, or flag lifecycle.
- `test-strategy` when verification is missing or insufficient.
- `systematic-debugging` when a finding needs a root cause rather than a guess.
- `performance-review` when cost is measurable and suspicious.
- `dependency-review` when dependencies change.
- `deployment-readiness` when the change is about to ship or touches deploy, environment, or migration safety.
- `incident-response` when the finding is live production impact rather than a code defect.

Do not run every skill because it exists. A CSS-only change does not need database review; a query change does not need frontend review.

## Read-only rule

During the initial review, do not edit files, create fixes, delete or format files, upgrade dependencies, change configuration, commit, push, merge, deploy, or mutate databases. Safe tests, lint, typecheck, build, and non-destructive diagnostics are allowed when useful.

## Evidence and severity

Base every finding on inspected code or an executed check. Distinguish confirmed issues from risks needing verification and label the latter `Needs verification`. Use Critical, High, Medium, Improvements, and Passed / No Issue Found. Avoid arbitrary numeric quality scores.

## Approval gate

End the report by stating that no project files were modified and asking whether to implement the findings. Wait for explicit approval such as `Fix finding #2` or `Implement critical findings only`. Never treat `okay`, `interesting`, `I see`, or `explain this` as approval. After approved fixes, rerun the relevant checks and report what remains.
