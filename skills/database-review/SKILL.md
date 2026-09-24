---
name: database-review
description: Review database, query, API/server-function, loader/action, and data-layer changes for correctness, performance, authorization, connection failures, and migration safety. Use whenever database queries/schema/data access change or cleanup touches backend data flow.
---
# Database Review

## Safety Default
Database interaction is read-only by default and restricted to SELECT queries against clone/dev databases unless the user explicitly grants write access for the task. Never INSERT, UPDATE, DELETE, ALTER, DROP, TRUNCATE, create tables, apply migrations, or perform destructive operations without explicit authorization.

Do not create/modify/run migrations as a side effect. Schema/migration work requires an explicit request. Prefer small, reviewable, reversible migrations and prepare them in code without applying to live databases unless explicitly requested. Never touch production credentials/secrets.

## Query and Data-Layer Review
For changed queries, APIs, server functions, loaders, actions, or fetching:
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
For database/API/network/external-service failures, avoid unnecessary crashes, handle rejected requests, provide appropriate user-facing failure/retry behavior, prevent destructive/misleading UI state, and preserve valid known data where sensible.

## Documentation
Keep schema/query changes synchronized with relevant architecture, decisions, modules, patterns, bugs, or WIP notes when reusable knowledge changes.
