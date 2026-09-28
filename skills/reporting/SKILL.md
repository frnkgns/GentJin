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
- Plain-language, scannable bullets starting with action verbs (Made, Added, Improved, Updated, Standardized, Fixed).
- Cover meaningful scope such as navigation, pages/forms, business rules, authorization, fixes, and verification.
- Avoid repetitive What Changed/Why/Impact boilerplate unless a detailed/technical report is explicitly requested.
- Group related edits by user-visible outcome, not by file. One outcome per bullet.

The vault report starts with `# <Report Title>` and date. It should be complete permanent documentation and include applicable scope, meaningful changes, files/modules, DB/API/UI/UX, responsive work, bugs/root causes, tests, type/lint/build status, performance, fallbacks, security, limitations, and follow-up. Keep all file paths, function/constant names, and implementation jargon in the vault report.

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

Console readability rules (apply to every `/report`):
- `Changes`: 5-9 bullets max. Group related edits into one outcome bullet. Merge minor polish into one `Minor UI polish` bullet or omit if not user-visible.
- Each bullet: under ~25 words, start with an action verb, describe what the reader can now do or see.
- Never include file paths, filenames, function/constant/component names, or implementation jargon (aria-*, debounce, infinite scroll, UTC, constants, helpers) in console. Translate internals to plain benefit.
- Keep Testing/Issues/Suggestions to one line each, plain language, no stack traces or code.

## Accuracy
Never claim work, testing, build success, or optimization that was not actually performed/verified. State unavailable verification clearly.
