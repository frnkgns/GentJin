---
name: knowledge-vault
description: Manage persistent project knowledge in the user's Obsidian vault. Use for project/vault discovery, note-first lookup, automatic note capture, vault bootstrap or migration, bugs, enhancements, questions, sessions, ADRs, architecture, patterns, modules, glossary, templates, and knowledge synchronization after meaningful development work.
---
# Knowledge Vault

## Project and Knowledge Discovery
1. Resolve the project root from the nearest Git root, project metadata, or working directory.
2. Resolve identity from `.project-agent.md`, explicit metadata, package/project name, Git repository name, or root directory name.
3. Normalize the identifier for filesystem use: lowercase, trim, spaces/underscores to hyphens, remove unsafe characters, collapse repeated hyphens.
4. Default knowledge root to `~/Documents/ObsidianVault/<project-identifier>/` unless `.project-agent.md` overrides it.
5. Keep the global configuration project-agnostic.

## Note-First Workflow
Before answering a project question or making significant code changes:
1. Search the vault by problem/scenario, not exact wording.
2. Read only relevant notes and meaningful wiki links.
3. Reuse a recorded solution when it still matches current code.
4. If a recorded solution no longer works, verify against source code, correct it, and update the note.
5. Inspect current source code before implementation decisions.

Priority: current source/schema -> accepted decisions -> architecture -> modules -> patterns -> concepts -> bug history -> WIP -> answered questions -> sessions -> reports.

## Canonical Vault Structure
Use the same structure at the project root and durable module/system knowledge directories:

```text
00-Home.md
inbox/
concepts/
architecture/
decisions/
modules/
patterns/
bugs/
enhancements/
glossary/
questions/
  archive/
work-in-progress/
  archive/
sessions/
_templates/
reports/
```

Create missing folders when bootstrapping; do not create empty notes merely to fill the structure.

## Automatic Capture
- Capture meaningful requests, bugs, fixes, decisions, patterns, and reusable lessons.
- If classification is unclear, capture in `inbox/` and triage later.
- Broken behavior -> `bugs/`.
- Non-bug improvement/polish requiring code -> `enhancements/`.
- Genuine unresolved investigation -> `questions/`.
- Architecture decisions -> `decisions/` as ADRs.
- Reusable implementation behavior -> `patterns/`, `concepts/`, `modules/`, or `architecture/` as appropriate.
- Search before creating; update an existing topic note instead of duplicating it.
- Put the user's request at the top of bug/enhancement notes, followed by location, applied change/root cause, reusable rule, and related links.

## Questions
Only create question notes for genuine investigation or when explicitly asked to save/revisit a question. Track status as Open, Investigating, or Answered. Investigate using current code and relevant knowledge, then update the answer and promote reusable knowledge to permanent notes. Archive answered questions when no longer active.

## Sessions and ADRs
- Create session notes only when historical context is useful beyond active WIP; never save full transcripts.
- ADRs contain Status, Context, Decision, Reasoning, Alternatives, Consequences, and Related notes.
- When an architectural decision changes, preserve prior reasoning and document why it changed.

## Vault Migration
When a canonical knowledge root is resolved, check for clearly equivalent older project vaults. Inspect source and destination, merge unique useful knowledge, preserve history/wiki links, verify the canonical vault, then remove the obsolete vault only when identity and migration are certain. Never delete when ambiguous.

## Templates and Quality
Use matching files in `_templates/` when present. Keep notes concise, current, topic-based, and reusable. Do not copy entire source files or large implementations; reference source files and use minimal snippets only when needed.
