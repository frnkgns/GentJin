---
name: refactoring-migration
description: Execute a large-scale refactor, migration, or architecture change safely across an existing codebase. Use when changing a data shape, moving a service or module, dual-writing, expand-contract, introducing or retiring a feature flag, deprecating an API or table, backfilling data, strangler-fig transitions, or changing a contract that existing consumers depend on. Owns the staged execution path; use architecture-review for the design decision and project-conventions for matching existing style.
---

# Refactoring Migration

A migration is a sequence of independently shippable steps, each of which leaves the system working. If you cannot describe the next step as safe to ship on its own, the plan is not ready.

## Before Starting

Confirm all of the following. Ask once for whatever is missing.

- **The end state is written down** — what the code looks like when this is finished, in concrete terms.
- **The steps are ordered** and each is independently reversible or at least independently survivable.
- **The blast radius is known** — which modules, callers, tables, jobs, or external consumers are affected. Search for every consumer; assume there is one you have not found.
- **Verification exists** — how each step is validated, and what "still works" means concretely.
- **Rollback exists** — for each step, what specifically you do to undo it.

If the end state is not agreed, stop. A migration without a defined destination drifts.

## Patterns

### Expand-contract

The safest default for changing a data shape or contract:

1. **Expand** — add the new form alongside the old. Both are valid. Nothing reads the new one yet.
2. **Migrate** — backfill or dual-write so both forms hold the same information. Verify they agree.
3. **Switch** — move readers to the new form. Keep the old one written as a fallback.
4. **Contract** — remove the old form only after the new one has been live and verified for a real duration.

Never combine step 3 and step 4. Removing the old form in the same release that stops reading it leaves no rollback.

### Dual-write

Write to both forms during migration. Dual-write is not dual-read: pick one form as authoritative for reads and state which.

Failures to watch: partial writes where one side succeeded, ordering between the two writes, and retries that duplicate one side. Make the operation idempotent, and make a partial write detectable and repairable.

### Feature flags

- A flag is a migration tool, not a permanent branch. Introduce it with a stated retirement condition and an owner.
- A flag that has served its purpose and is now permanently on is dead weight. Remove it and the dead branch.
- Never stack many flags on one path; that makes every future change a combinatorial problem.
- Do not use a flag to avoid deciding. If the decision is "which design wins", that decision is still owed.

### Strangler pattern

When replacing a service or module incrementally, route around the old one rather than rewriting it. Introduce a boundary, move one caller at a time, and delete the old path once no caller remains.

## Data Migration

- Prefer additive, backward-compatible schema changes while the previous code version still runs.
- A non-backward-compatible migration requires that the old version no longer be serving before it executes. Sequence accordingly.
- Backfills run in batches with a rate limit, are resumable, and are idempotent. A backfill that cannot be restarted after a crash is a batch job that will eventually fail.
- Verify with a reconciliation query that old and new agree, and report the count of rows that do not.

## Per-Step Discipline

Every step, no matter how small:

1. It compiles, passes typecheck, and passes the relevant tests.
2. It is separately shippable and separately revertable.
3. Its verification was actually run, not assumed.
4. Its effect is recorded in the Knowledge Vault when it establishes a durable constraint.

Report progress as steps completed and verified, never as percentage of a plan.

## Stop Conditions

Stop and ask when:

- a step turns out not to be independently safe after implementation detail emerged;
- an unforeseen consumer of the old form is found;
- verification fails and the cause is not understood — use `systematic-debugging`;
- the plan would require shipping a non-backward-compatible change without a coordinated cutover.

## Retiring What You Replaced

The migration is not finished when the new path works. It is finished when the old path is gone:

- Find and remove every remaining reference to the old form, including dead code, tests, docs, config, and comments.
- Remove the compatibility shim once no caller needs it.
- Remove the flag once its condition is met.
- Search the codebase to prove the old path has no remaining references, and report the evidence.

## Ownership

Keep the staged execution path, data migration mechanics, flag lifecycle, dual-write safety, and retirement here. Use `architecture-review` for the structural decision before you start, `project-conventions` to match existing patterns, `database-review` for schema and backfill correctness, `api-design` for contract shape and versioning, `test-strategy` for coverage across each step, `test-strategy` and `systematic-debugging` when a step fails, and `deployment-readiness` before shipping each step.
