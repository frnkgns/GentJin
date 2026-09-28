---
name: deployment-readiness
description: Assess whether a project is safe to ship to production. Use when the user asks about deployment, release, production readiness, going live, launch safety, environment or config parity, CI/CD pipelines, rollback plans, migration safety before shipping, or secret and credential exposure. Also use when release or deployment-sensitive work is being finished. Delegate per-layer code correctness to the review skills; this skill owns the ship-readiness question.
---

# Deployment Readiness

Answer one question: is it safe to ship this to production right now?

Produce an evidence-based verdict, not a reassurance. Report `READY`, `READY WITH WARNINGS`, or `BLOCKED`, then the evidence for each.

## Scope

Inspect the current project and the change in front of it. Determine what actually ships: the branch, the diff, the build artifacts, the environment configuration, and the data layer.

Record unrelated existing files only when the deployment touches them. Do not broaden into a general code review; use the review skills for that.

## Checks

### Build and CI

- Does the project's real build command succeed from a clean state? Run it; do not assume.
- Do typecheck and lint run in CI, and do they pass on the current branch?
- Do tests run in CI, and are any skipped, flaky, or failing?
- Are CI failures pre-existing on the base branch, or introduced by this change? Distinguish them; a pre-existing failure is a warning, not a new blocker.
- Are secrets, tokens, or credentials reachable from the build context, the bundle, or the client payload?

### Environment parity

- Which environment variables, feature flags, and config values does the app read? Compare the local values against what production provides.
- Is every required variable defined somewhere deployable, and is every one actually read at runtime?
- Are environment-specific values hardcoded anywhere instead of coming from config?
- Does a missing variable fail loudly at startup, or silently at the first user request?

### Data layer

- Does the change include a schema or data migration? If so, is it reversible, and is it safe to run while the previous version of the app is still serving traffic?
- Is the migration additive and backward compatible, or does it break the currently deployed code?
- Is a backfill required, and is it reversible?
- Is there a dry run, or a way to verify the migration before it executes in production?

### Runtime and operations

- How is the app started, and does that command exist and work as configured?
- Is there a health check that reflects real readiness rather than process liveness?
- Is logging sufficient to diagnose a failure after release, without logging secrets or personal data?
- What is the rollback procedure, and has it been exercised?
- Are long-running processes, background jobs, or scheduled tasks accounted for?

### Dependencies and supply chain

- Were dependencies added or upgraded? Use `dependency-review` for advisory and install-script risk.
- Is the lockfile committed and consistent with the manifest?
- Are there known vulnerabilities in direct dependencies, and do they affect reachable code?

## Verdict

```text
# DEPLOYMENT READINESS

Target: <branch / commit / scope>

## VERDICT
READY | READY WITH WARNINGS | BLOCKED

## BLOCKERS
1. ...

## WARNINGS
1. ...

## PASSED
1. ...

## EVIDENCE
- <check> → <result>

## ROLLBACK PLAN
...

## NOT VERIFIED
- <check that could not be run, and why>
```

Every blocker and warning must cite evidence from an inspected file or an executed command. Label anything unverified as `Needs verification` and list it under `NOT VERIFIED`. Never claim a build, test, or migration succeeded when it did not run.

## Boundaries

This skill reports; it does not change. Do not edit configuration, commit, tag, push, deploy, run migrations against production, or alter CI/CD definitions. Ask for approval before any action that ships.

## Ownership

Keep the ship-readiness verdict, environment parity, migration safety, and rollback planning here. Delegate per-layer correctness to `frontend-review`, `backend-review`, `database-review`, `security-review`, `integration-review`, and `performance-review`; delegate dependency risk to `dependency-review`; delegate test gaps to `test-strategy`. Use `systematic-debugging` if a check fails and the cause is unknown.
