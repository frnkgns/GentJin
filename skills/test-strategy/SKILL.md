---
name: test-strategy
description: Decide what should be tested and at which level before or during implementation. Use when implementing a meaningful feature, creating or changing tests, touching payment/auth/data-integrity logic, when a review finds insufficient verification, or when the user asks what needs testing. For an unexplained failure or regression, load `systematic-debugging` first to find the cause, then this skill to cover the fix.
---
# Test Strategy

## Principle

Risk-driven, not coverage-driven. Do not optimize for test count or arbitrary coverage percentages.

## Choose the right level

- Unit: pure logic, calculations, validation, transformations, state transitions.
- Integration: real module boundaries, database queries, service calls, transactions.
- Contract: request/response shapes and consumer expectations between components or teams.
- API: routing, auth, validation, status codes, and error contracts on endpoints.
- End-to-end: critical user journeys only, kept few and stable.
- Smoke: post-change or pre-release confidence that the app still starts and core paths work.
- Manual QA: flows automation cannot judge reliably, such as visual layout, device behavior, or third-party dashboards.
- Regression: previously fixed defects that must stay fixed.

## Identify what to cover

- Happy paths.
- Edge cases: boundaries, empty and maximum values, ordering, timezones, encoding.
- Invalid input: malformed, oversized, unexpected types, and hostile input.
- Permission boundaries: unauthorized, wrong role, and cross-user access.
- Failure and retry behavior: timeouts, partial failure, retries, and recovery.
- Concurrency where relevant: races, parallel writes, ordering guarantees.
- Duplicate events and webhooks where relevant: idempotency and replay.
- Rollback and recovery where relevant for risky flows.

## Rules

- Match the project's existing test framework, file layout, and naming; do not introduce a second framework without a reason.
- Test observable behavior and contracts, not private implementation details.
- Keep tests deterministic; control time, randomness, network, and ordering.
- Do not weaken or delete an existing test to make a suite pass; fix the cause with `systematic-debugging` when a test fails for the wrong reason.
- Do not write tests against production or shared resources. Skip unsafe tests and say so.
- Prefer a few high-value tests over many shallow ones.

## Reporting

State which levels were chosen and why, what was added or changed, what was deliberately not covered, and the exact commands that were run. Never claim coverage or passing tests without executing them.
