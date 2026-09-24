---
name: reporting
description: Create project change, implementation, QA, debug, review, or cleanup reports. Use when the user asks create/write/generate/prepare a report, document changes, summarize implementation changes, or when cleanup requires its mandatory final report. Uses Git/session/vault evidence and produces a vault report plus concise console report.
---
# Reporting

## Report vs Cleanup
A standalone report request is report-only: do not run cleanup first. `clean up` performs cleanup and then invokes this reporting workflow.

## Evidence
Determine report scope using strongest evidence first:
1. Pending source-control changes from `git status` and `git diff`.
2. If clean/already committed/not Git, use session evidence: requested task, files changed, WIP/session/vault notes, decisions, and actual verification.
3. If no verifiable changes exist, say so; never invent work or results.

## Title and File
Derive a short title from the feature/system/backlog item. Use the backlog item's name when known. Use the exact same title in vault and console outputs. Follow an existing project report naming convention; otherwise use `YYYY-MM-DD - <Report Title>.md` under `reports/`.

## Default Report Style
Use a simple implementation-list style:
- Plain-language, scannable bullets starting with action verbs.
- Cover meaningful scope such as navigation, pages/forms, business rules, authorization, fixes, and verification.
- Avoid repetitive What Changed/Why/Impact boilerplate unless a detailed/technical report is explicitly requested.

The vault report starts with `# <Report Title>` and date. It should be complete permanent documentation and include applicable scope, meaningful changes, files/modules, DB/API/UI/UX, responsive work, bugs/root causes, tests, type/lint/build status, performance, fallbacks, security, limitations, and follow-up.

## Console Report
Print a concise version, not the full vault report:

```text
Title: <Report Title>

Changes
1. ...

Testing
1. ...

Issues
1. ...

Suggestions
1. ...
```

`Changes` is required. Include Testing only if checks ran, Issues only for unresolved limitations, and Suggestions only when useful.

## Accuracy
Never claim work, testing, build success, or optimization that was not actually performed/verified. State unavailable verification clearly.
