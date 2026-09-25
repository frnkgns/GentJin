---
name: knowledge-vault
description: Manage persistent project knowledge in the user's Knowledge Vault. Use for project/vault discovery, note-first lookup, vault initialization and normalization, automatic knowledge capture, architecture and ADRs, reusable concepts and patterns, bugs, enhancements, investigations, work-in-progress, sessions, reports, and migration of existing project knowledge. Also use for workflow memory: searching reusable task procedures before repeating a personal or project task, and capturing or updating a workflow note after a recurring task succeeds, or when the user says "remember this", "save this workflow", "how do I open or run X again", or "don't do that again".
---

# Knowledge Vault

## Project Discovery

1. Resolve the project root from the nearest Git root, project metadata, or working directory.
2. Resolve the project identity in this order:
   - `.project-agent.md`
   - explicit project metadata
   - package/project name
   - Git repository name
   - root directory name
3. Normalize the identifier for filesystem use:
   - lowercase
   - trim whitespace
   - convert spaces and underscores to hyphens
   - remove unsafe characters
   - collapse repeated hyphens
4. Resolve the knowledge root to:

   `~/Documents/KnowledgeVault/<project-identifier>/`

   unless `.project-agent.md` explicitly overrides it.

5. Keep global GENTJIN configuration project-agnostic.

## Workflow Memory

Use workflow memory for recurring commands, personal automation patterns, and repeatable computer tasks that are likely to be requested again. Keep cross-project workflows in the global GENTJIN workflow store and project-specific workflows in the current project vault.

Use `~/Documents/KnowledgeVault/gentjin/workflows/` as the default global workflow store unless project metadata provides an explicit override. Use `<project-knowledge-root>/workflows/` for project-specific workflows.

Before acting on a recurring request:

1. Search by intent, target, and expected outcome rather than exact wording.
2. Read the smallest relevant workflow note.
3. Check its scope, preconditions, status, and last-verified date.
4. Reuse it only when the current project, environment, tools, and permissions still match.
5. Treat current source code, current tool behavior, and explicit user instructions as authoritative when older knowledge conflicts.

After a workflow succeeds, capture it only when it has likely future value. Keep one concise note with:

- Status
- Scope
- Trigger phrases
- Intent
- Preconditions
- Steps
- Verification
- Failure or revision notes
- Last verified date

If the user reports that a workflow did not work, do not mark it verified or create a duplicate. Preserve the failure reason, revise the existing note, and record the replacement only after verification. Never store full transcripts, credentials, secrets, one-off requests, or unverified guesses.

## Knowledge Vault Migration

When the legacy `~/Documents/ObsidianVault/` directory exists and `~/Documents/KnowledgeVault/` does not, and the user explicitly authorizes the migration:

1. Verify the source is a directory and the destination does not exist.
2. Rename only the vault directory; preserve all notes and hidden vault metadata.
3. Update only the Obsidian application's saved vault path when explicitly authorized.
4. Never rename, move, or modify the Obsidian application installation or unrelated application folders.
5. Verify the destination, note count, and saved path after the rename.

If the destination already exists, do not merge directories automatically; report the conflict and ask before proceeding. GENTJIN installation may perform this same guarded migration only after explicit user approval, and only when Obsidian is closed.

## Vault Initialization

GENTJIN uses one canonical vault architecture.

If the project vault does not exist, initialize the canonical structure automatically.

If it already exists:

- Preserve all existing knowledge.
- Never reset or recreate the vault.
- Create missing canonical directories when needed.
- Preserve additional user-created directories and notes.
- Never delete or overwrite existing knowledge merely to normalize structure.

Do not create empty notes solely to populate directories.

## Canonical Vault Structure

```text
<project-identifier>/
├── 00-Home.md
│
├── architecture/
│   ├── modules/
│   └── decisions/
│
├── knowledge/
│   ├── concepts/
│   ├── patterns/
│   └── glossary.md
│
├── issues/
│   ├── bugs/
│   ├── enhancements/
│   └── questions/
│       └── archive/
│
├── work-in-progress/
│   └── archive/
│
├── sessions/
├── reports/
├── workflows/
└── _templates/
```

Use this architecture consistently across GENTJIN-managed project vaults.

Do not create deeper directory structures unless the project's scale clearly requires them.

## Note-First Workflow

Before answering a project-specific question or making significant implementation changes:

1. Identify the problem or development area.
2. Search relevant project knowledge by meaning or scenario rather than exact wording.
3. Read only the notes relevant to the current task and their meaningful wiki links.
4. Inspect current source code and schemas.
5. Reuse recorded decisions, patterns, or solutions when they still match the current implementation.
6. If stored knowledge conflicts with current source code, treat the current implementation as authoritative.
7. Correct outdated knowledge after verifying the new behavior.

Do not load the entire vault.

Prefer:

`Search → Relevant Notes → Current Source/Schema → Work`

Knowledge priority:

`Current Source/Schema → Accepted ADRs → Architecture → Modules → Patterns → Concepts → Issue History → Active WIP → Questions → Sessions → Reports`

## Knowledge Routing

Route knowledge according to its purpose.

### Architecture

Use `architecture/` for documentation describing how the system currently works.

Examples:

- system architecture
- authentication architecture
- data flow
- API structure
- infrastructure
- major subsystem relationships

Use:

`architecture/modules/`

for durable knowledge about specific modules, domains, or major features.

Use:

`architecture/decisions/`

for Architecture Decision Records (ADRs).

### Knowledge

Use:

`knowledge/concepts/`

for important project-specific concepts, terminology, business rules, or domain behavior.

Use:

`knowledge/patterns/`

for reusable implementation patterns, conventions, solutions, and lessons.

Use:

`knowledge/glossary.md`

for concise definitions of recurring project-specific terminology.

### Issues

Use:

`issues/bugs/`

for confirmed broken behavior, regressions, root causes, and verified fixes.

Use:

`issues/enhancements/`

for requested improvements, refinements, optimizations, and non-bug development work.

Use:

`issues/questions/`

only for genuine unresolved investigations.

Do not create a question note for something that can be immediately determined from the current source code.

### Work in Progress

Use:

`work-in-progress/`

for active development state that must survive between sessions.

Examples:

- partially implemented features
- unfinished debugging
- pending verification
- multi-step refactors
- paused development work

Move completed or abandoned WIP notes to:

`work-in-progress/archive/`

when historical context remains useful.

### Sessions

Use `sessions/` only when historical development context is useful beyond active WIP.

Do not save full conversations or transcripts.

Prefer concise summaries containing:

- goal
- meaningful changes
- discoveries
- unresolved items
- relevant links

### Reports

Use `reports/` for generated development reports, audits, reviews, or requested summaries that should remain part of project history.

## Automatic Capture

Capture knowledge when it has likely future value.

Good candidates include:

- architectural decisions
- important implementation decisions
- confirmed bugs and their root causes
- reusable fixes
- recurring implementation patterns
- important project concepts
- non-obvious business rules
- meaningful enhancements
- unresolved investigations
- significant WIP state

Do not create notes for trivial edits, obvious implementation details, temporary observations, or information easily recovered from source code.

Search before creating a note.

If an appropriate topic note already exists, update it instead of creating a duplicate.

Prefer one durable topic note over many small notes describing the same subject.

## Bug and Enhancement Notes

For bug and enhancement notes, preserve the user's original intent near the beginning of the note.

Include only useful sections such as:

- Request / Problem
- Location
- Root Cause
- Change
- Verification
- Reusable Lesson
- Related Notes

Do not force sections that provide no useful information.

A resolved bug remains in `issues/bugs/` because its root cause and solution may be valuable later.

Do not archive bug history merely because the issue is resolved.

## Questions

Create a question note only when:

- investigation must continue later;
- the answer cannot yet be determined confidently; or
- the user explicitly asks to preserve the question.

Use statuses:

- `Open`
- `Investigating`
- `Answered`

When answered:

1. Record the answer.
2. Promote reusable findings to architecture, knowledge, patterns, or another appropriate permanent note when useful.
3. Move the question to `issues/questions/archive/` when it is no longer active.

Do not duplicate the complete answer across several notes.

## Architecture Decision Records

Store ADRs under:

`architecture/decisions/`

Use ADRs for decisions that materially affect architecture, technology, data design, infrastructure, security, or long-term implementation direction.

An ADR should normally contain:

```text
Status
Context
Decision
Reasoning
Alternatives
Consequences
Related
```

Supported statuses should include:

- Proposed
- Accepted
- Superseded
- Deprecated

Do not rewrite history when a decision changes.

Instead:

1. Preserve the original ADR.
2. Mark it `Superseded` when appropriate.
3. Create or link the newer ADR.
4. Document why the decision changed.

Architecture describes **how the system works**.

ADRs describe **why important architectural choices were made**.

## Home Note

Use `00-Home.md` as the lightweight entry point into project knowledge.

Keep it concise.

It may link to:

- architecture overview
- major modules
- important ADRs
- active WIP
- important project concepts
- current investigations

Do not turn `00-Home.md` into a duplicate of the entire vault.

## Templates

Use `_templates/` for reusable note structures when templates materially improve consistency.

Templates may exist for:

- ADRs
- bugs
- enhancements
- questions
- WIP
- sessions
- reports

Do not require every note to use a template when a simpler note is sufficient.

## Vault Migration and Normalization

When resolving a project vault:

1. Check whether the canonical vault already exists.
2. Check for clearly equivalent older project vaults only when there is evidence that one exists.
3. Never migrate based only on a similar directory name.
4. Inspect source and destination before migration.
5. Preserve unique useful knowledge and wiki links.
6. Normalize older structures into the current canonical architecture when safe.

Examples:

```text
decisions/     → architecture/decisions/
modules/       → architecture/modules/
concepts/      → knowledge/concepts/
patterns/      → knowledge/patterns/
glossary/      → knowledge/glossary.md
bugs/          → issues/bugs/
enhancements/  → issues/enhancements/
questions/     → issues/questions/
```

Do not blindly move files when equivalent destination knowledge already exists.

Merge knowledge when appropriate instead of creating duplicates.

Never delete an older vault until:

- project identity is certain;
- useful knowledge has been migrated;
- links and important references have been preserved;
- the canonical vault has been verified.

If identity or migration safety is ambiguous, preserve both and report the ambiguity.

## Knowledge Synchronization

After meaningful development work:

1. Determine whether durable project knowledge changed.
2. Update existing notes before creating new ones.
3. Record newly discovered reusable knowledge.
4. Update relevant architecture when system behavior changed.
5. Create or update an ADR when an architectural decision changed.
6. Update bug/enhancement notes with verified outcomes.
7. Update or archive WIP when work reaches a stable state.
8. Keep links between related knowledge when useful.

Do not update the vault merely to record that files were edited.

Capture the **reason, behavior, decision, or reusable lesson**, not a duplicate Git history.

## Quality Rules

Keep project knowledge:

- concise
- topic-based
- searchable
- current
- reusable
- linked when relationships are meaningful

Prefer explaining:

**what → why → consequence**

rather than recording every implementation step.

Do not:

- copy entire source files into notes;
- store large code implementations;
- save full conversations;
- duplicate Git history;
- create duplicate topic notes;
- create notes solely because a folder exists;
- load the entire vault for every task.

Reference source files and use minimal snippets only when they materially help explain the knowledge.

Current source code and schemas remain the primary evidence for current implementation behavior.
