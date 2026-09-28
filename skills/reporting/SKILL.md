---
name: reporting
description: Create project change, implementation, QA, debug, review, or cleanup reports. Use when the user asks create/write/generate/prepare a report, document changes, summarize implementation changes, or when cleanup requires its mandatory final report. Uses Git/session/vault evidence and produces a vault report plus concise console report.
---
# Reporting

## Report vs Cleanup
A standalone report request is report-only: do not run cleanup first. `clean up` performs cleanup and then invokes this reporting workflow.

## Capture Before Reporting

Never write the report until the note sweep has run. This is a required step, not advice.

1. List the note types this work could have touched: project structure, architecture, a decision, a durable bug or root cause, a feature, discovered conventions, current WIP, today's session, a change note, a rejected attempt, an open question.
2. Check each one against what actually changed. Update the ones whose durable content changed. Create any that are missing. Correct any that are stale.
3. If a decision was made and has no note, write it before the report. If an approach was rejected and would be re-attempted, record why it failed.
4. If nothing durable was established, state that explicitly rather than inventing a note.

Do not ask the user for permission to capture. Report what was captured in the `Notes` section.

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

Notes
1. <note created, updated, or corrected> — <why it was durable>
```

`Changes` is required. `Notes` is required. Include Testing only if checks ran, Issues only for unresolved limitations.

`Notes` must list every vault note this work created, updated, or corrected, with the durable fact each one now holds. If genuinely nothing durable was established, write `None — no durable knowledge established` and say why in one clause. A missing `Notes` section is a defect in the report, not a style choice. Never report completion with `Notes` unstated.

Console readability rules (apply to every `/report`):
- `Changes`: 5-9 bullets max. Group related edits into one outcome bullet. Merge minor polish into one `Minor UI polish` bullet or omit if not user-visible.
- Each bullet: under ~25 words, start with an action verb, describe what the reader can now do or see.
- Never include file paths, filenames, function/constant/component names, or implementation jargon (aria-*, debounce, infinite scroll, UTC, constants, helpers) in console. Translate internals to plain benefit.
- Keep Testing/Issues/Suggestions to one line each, plain language, no stack traces or code.
- `Notes`: name the note by its plain-language subject, not its path. `Recorded the autocrlf divergence and its fix`, not `issues/bugs/2026-09-28 - Git autocrlf.md`.

## Accuracy
Never claim work, testing, build success, or optimization that was not actually performed/verified. State unavailable verification clearly.
