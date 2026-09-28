---
name: database-review
description: Review database, query, schema, migration, and data-layer changes for correctness, query-level performance, authorization, data integrity, and connection failures. MUST be used before creating or modifying schema or migrations, before any database write, and whenever queries or data access change, including during cleanup. Not for handler, service, job, retry, or idempotency concerns; use `backend-review` for those.
---

# Database Review

## Safety Default

The global `## Safety` rules in AGENTS.md govern database access: read-only SELECT against clone or dev databases by default, and no write, DDL, migration, or destructive operation without explicit authorization. Schema or migration work requires an explicit request, and production credentials and secrets are never touched.

Prefer small, reviewable, reversible migrations, prepared in code rather than applied to a live database unless explicitly requested.

## Query and Data-Layer Review

For changed queries, loaders, actions, or the data-access portion of a fetching path:

- Avoid N+1 queries and duplicate requests.
- Avoid unnecessary columns/records.
- Use appropriate filtering, pagination, joins, indexes, caching, batching, or parallelization when justified.
- Reuse data already available in the request/component flow.
- Avoid unnecessary sequential blocking.
- Verify query correctness, ownership, and authorization constraints.
- Do not prematurely optimize unaffected code.

## Data Integrity

- Validate inputs.
- Ensure mutations cannot create duplicate/inconsistent records.
- Verify derived totals/counts/statuses use the correct authoritative records.
- Never replace failed data with fake/default business values that imply success.

## Failure Handling

For database, API, network, or external-service failures:

- Never expose raw database errors, SQL errors, stack traces, exception details, internal paths, or sensitive implementation details to end users.
- Show a safe, user-friendly fallback message such as `Something went wrong. Please try again.` when no more specific actionable message is appropriate.
- Use specific user-facing messages when they are safe and useful, such as `Unable to load records. Please try again.`
- Preserve the original technical error for server-side logging/debugging where appropriate.
- Handle rejected requests and unexpected responses without unnecessary application crashes.
- Provide retry behavior when the operation can safely be retried.
- Prevent destructive, stale, or misleading UI state after a failed operation.
- Preserve previously valid data when sensible instead of replacing it with fake/default business values.
- Never present a failed operation as successful.

## Documentation

Keep schema/query changes synchronized with relevant architecture, decisions, modules, patterns, bugs, or WIP notes when reusable knowledge changes.

## Related Skills
Keep query correctness, data integrity, schema behavior, query-level performance, migration safety, and read/write safety here. Use `backend-review` for handler and service concerns such as server functions, retries, idempotency, and resource cleanup; `api-design` for error-contract shape and versioning; `performance-review` for cross-stack measurement and caching methodology; `dependency-review` for ORM or driver changes.
