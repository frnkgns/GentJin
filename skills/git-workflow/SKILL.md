---
name: git-workflow
description: Handle guarded Git and source-control workflows. MUST be used before any commit, branch creation, push, merge, rebase, or pull request. Use when inspecting repository status/history/diffs, matching repository commit and branch conventions, or when cleanup/reporting needs source-control evidence. Never performs mutating Git actions unless explicitly requested.
---
# Git Workflow

## Inspect Before Acting
Before a requested commit or source-control change:
- Inspect `git status`.
- Inspect relevant `git diff` (staged and unstaged as applicable).
- Inspect recent history to match repository conventions.
- Understand what belongs to the logical change.

## Repository Conventions
Detect and follow the repository's own branch naming format rather than inventing one. If it has none, use the default from the global `## GitHub Pushes` rules and continue its sequence. Stage only the files belonging to the logical change.

## Jira Backlog Title
When the branch name matches `^[A-Z]+-\d+-(.+)$`, derive the Jira title from the description: split on `-`, uppercase any word of two characters or fewer, capitalize the rest, and join with spaces.

```text
GX-33-be-fix-sales-journey-medicare-admission-count
  → BE Fix Sales Journey Medicare Admission Count
```

Do not derive a title from a branch that does not match, or from a protected branch. Always show the derived title and let the user confirm or correct it before it is used.

The `/git-push` command implements this workflow end to end.

## Commits
Create one focused commit per logical unit. Prefer a short imperative subject, blank line, and concise body explaining why when useful. Match existing repository commit style when present.

## Knowledge Sync
After meaningful work, ensure relevant WIP/vault knowledge reflects what is being committed. Do not claim source-control state without checking it.
