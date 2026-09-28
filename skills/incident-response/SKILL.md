---
name: incident-response
description: Diagnose and resolve production incidents where users are already affected. Use when something is broken in production, error rates spiked, an outage or degraded service is reported, a deploy went wrong, customers report failures, or the user asks to roll back, hotfix, or restore service. Pairs with systematic-debugging for the root cause but owns the live-impact and mitigation path.
---

# Incident Response

When users are already affected, the first job is to stop the bleeding, not to understand the code. Stabilize, then diagnose, then fix properly.

## Severity First

Classify before acting. Ask only what is needed to classify; do not block on a perfect answer.

- **SEV1** — widespread outage, data loss or corruption, or security breach. Every mitigation path is fair, including rollback.
- **SEV2** — major feature unavailable or severely degraded for a large share of users.
- **SEV3** — limited impact, workaround exists.

Say which one you believe it is and why. If unsure, state the assumption and proceed with the safer action.

## Stabilize First

In order, prefer the fastest path to safety:

1. **Roll back** the most recent deploy or change if it correlates with onset. A rollback restores service faster than a diagnosis.
2. **Disable the feature** behind a flag, or kill the failing job, process, or consumer.
3. **Reduce load** — shed traffic, rate-limit, or pause a job hammering a struggling dependency.
4. **Restore data** only if corruption is confirmed, and only from a known-good backup or replay.

Announce the mitigation and its expected effect before performing it. Never perform a destructive mitigation on an uncertain target without explicit approval.

## Then Diagnose

After service is stable, work the cause with `systematic-debugging`. Establish:

- **Timeline** — when did it start, and what changed immediately before that: deploy, config change, migration, dependency update, traffic shift, or data change?
- **Blast radius** — which users, tenants, regions, or endpoints are affected? What is the actual error rate, not an impression?
- **Evidence** — logs, traces, metrics, and error reports. Read them; do not guess from the symptom description.
- **Reproducibility** — can it be reproduced outside production? If not, say so explicitly rather than claiming a fix is verified.

Prefer correlation with a concrete change over speculation about an unknown cause. "Started 14:02, deploy finished 13:58" is a finding. "Might be a race condition" is a hypothesis, and must be labeled as one.

## Communicate

Report concisely and factually:

- what is affected and how badly
- what was done to stabilize it
- what is still unknown
- the next action

Never present a mitigation as a fix. Never claim the cause is known when it is a hypothesis.

## Fix Properly

After stabilizing, treat the incident as real work, not a patch:

- Fix the root cause with `systematic-debugging`; do not weaken or disable a failing check to make it pass.
- Add or extend a regression test with `test-strategy` so the same failure is caught automatically next time.
- If the fix requires a risky change, stage it: flag, canary, or partial rollout, per `deployment-readiness`.
- Record the incident, timeline, root cause, and the fix in the Knowledge Vault. An incident whose cause is not written down will recur.

## Hotfix Rules

A hotfix is a deliberate exception to normal rigor, and it is temporary:

- Keep it minimal and surgical; do not refactor while stabilizing.
- Record it immediately in `attempts/` with why the full fix was not safe to ship at that moment.
- Track the follow-up to completion. An untracked hotfix becomes permanent accidental architecture.

## Ownership

Keep live-impact assessment, mitigation, rollback decisions, timeline, and hotfix discipline here. Use `systematic-debugging` for root cause, `deployment-readiness` for the ship decision on the fix, `database-review` for data corruption and recovery, `security-review` for a suspected breach, `backend-review` and `integration-review` for the failing code path, and `security-review` again before closing if credentials or personal data were involved.
