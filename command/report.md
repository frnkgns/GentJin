---
description: Create a change report (vault report + concise console report). Report only; no cleanup performed.
---

Create a report per the `reporting` skill. Report only — do not perform a cleanup review.

Determine what counts as "the changes", strongest evidence first:

1. Source-control changes: inspect `git status` and `git diff` when the directory is a Git repository. Pending staged or unstaged changes are the primary subject.
2. The session's work when source control has nothing pending: if the tree is clean or the directory is not a Git repository, base the report on the task the user asked for, the files touched, and the notes captured in the vault (WIP, session notes, inbox, bugs, enhancements, decisions).
3. If no verifiable changes exist, state that explicitly instead of inventing content.

Produce two outputs with the same title: a detailed report saved into the vault under `reports/`, and a concise console report following the `## Response Format` section of the global instructions.
