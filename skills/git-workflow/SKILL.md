---
name: git-workflow
description: Handle guarded Git and source-control workflows. Use when inspecting repository status/history/diffs, creating branches or commits, pushing, merging, preparing pull requests, or when cleanup/reporting needs source-control evidence. Never performs mutating Git actions unless explicitly requested.
---
# Git Workflow

## Safe Default
Only commit, push, create/switch branches, merge, submit pull requests, amend, rebase, rewrite history, force-push, or skip hooks when the user explicitly requests the relevant action.

## Inspect Before Acting
Before a requested commit or source-control change:
- Inspect `git status`.
- Inspect relevant `git diff` (staged and unstaged as applicable).
- Inspect recent history to match repository conventions.
- Understand what belongs to the logical change.

## Branches
Default naming when the repository has no stronger convention:
- `feature/<short-name>`
- `fix/<short-name>`
- `refactor/<short-name>`
- `chore/<short-name>`

## Commits
Create one focused commit per logical unit. Prefer a short imperative subject, blank line, and concise body explaining why when useful. Match existing repository commit style when present.

Never commit secrets, credentials, environment files, or generated artifacts that should not be versioned.

## Knowledge Sync
After meaningful work, ensure relevant WIP/vault knowledge reflects what is being committed. Do not claim source-control state without checking it.
