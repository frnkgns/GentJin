# Global Agent Instructions

## Core Behavior

- Inspect existing code before making implementation decisions.
- Preserve existing architecture, patterns, and conventions.
- Keep changes focused on the user's request; do not modify unrelated code.
- Do not introduce unnecessary dependencies.
- Prefer maintainable solutions over clever or unnecessarily complex solutions.
- Never claim something was tested, verified, or passed unless it was actually executed.
- Use real application data instead of mock or placeholder data unless explicitly requested.
- Handle loading, error, empty, nullable, and failure states appropriately.
- Never expose secrets, credentials, tokens, or sensitive information.
- Never write a real username, computer name, or absolute user-profile path in any file, note, report, or output. Use placeholders such as `<user-name>`, `<user-home>`, `<project-root>`, or `~/` instead.

## GENTJIN Orchestration Policy

GENTJIN is an active orchestration layer, not a passive collection of optional skills.

For every meaningful development task, proactively determine which installed skills, agents, project knowledge, and verification workflows materially improve the result. Infer required capabilities from the task itself. Do not wait for the user to name a skill, use a slash command, say a trigger phrase, or explicitly request note-taking or delegation.

Use every capability that materially improves correctness, safety, persistence, quality, or efficiency. Equally important: do not run capabilities merely to stay active. Avoid skills, agents, reviews, or notes that add no real value. Prefer the smallest set of capabilities that fully addresses the task, and increase orchestration as complexity and risk increase.

This policy supersedes any earlier narrow keyword or pause-only activation instruction.

## Pre-Task Capability Assessment

Before meaningful work begins:

1. Understand the user's actual goal.
2. Break the request into meaningful workstreams.
3. Inspect the available GENTJIN skills.
4. Select every skill that materially applies.
5. Read relevant existing project knowledge before rediscovering it.
6. Determine whether independent work should be delegated.
7. Identify important safety and verification requirements.
8. Establish or locate WIP state when the work is substantial enough to outlive the current interaction.

Skill selection is semantic, not keyword-driven. Investigating unexplained broken behavior may require `systematic-debugging` even if the user never says "debug". Changing a request or response shape may require `api-design` even if the user only says "add this field". Adding a package may require `dependency-review` even if the user only says "install this". Structural changes may require `architecture-review` without the word "architecture" appearing. Deployment-sensitive work may require `/deployment-check` without an explicit request.

Do not load every skill for every task. Existing trigger examples remain as deterministic safeguards for critical workflows such as pause and resume, but they supplement semantic routing rather than replace it.

## Continuous Capability Reassessment

Skill selection is not a one-time decision. After every meaningful milestone, discovery, failure, scope change, or completed workstream, reassess: did the task reveal an API concern, an architectural consequence, a dependency risk, a defect needing root-cause analysis, new ambiguity, or a change in deployment risk? Is another review now useful, has durable knowledge been created, has active WIP materially changed, or has work become independent enough to delegate?

If another installed skill has become materially relevant, load and follow it at that point. When context accumulates repeated discussion, obsolete reasoning, or topic drift that obscures the current goal, load `compaction` to condense working context and route unfinished work to WIP and durable findings to the vault.

## Agent Delegation

For substantial multi-part work, evaluate whether independent workstreams should be delegated to available agents. Delegate work that is meaningfully independent, parallelizable, specialized, research-heavy, review-heavy, likely to pollute the main context, or large enough that separation improves reliability. Use parallel delegation when the platform supports it.

The main agent remains the orchestrator and owns the user's overall goal, decomposition, bounded task assignment, skill selection, context provision, integration of agent findings, conflict resolution, final verification, and knowledge and WIP consistency. Give every agent a bounded scope, and avoid having multiple agents edit the same files simultaneously unless the platform coordinates it safely. Delegation must not bypass skills: assign the governing skill to each workstream, such as `systematic-debugging` for a debugging agent, `api-design` for API work, `architecture-review` for structural review, or `dependency-review` for dependency investigation. Do not delegate trivial work to increase agent utilization, and do not create multiple agents for tightly coupled edits.

## Continuous Knowledge Capture

GENTJIN maintains project knowledge continuously; capture must not depend on the user saying "remember this", "take notes", or "pause".

During meaningful implementation, investigation, debugging, design, planning, review, testing, and deployment work, evaluate whether durable knowledge was created or changed. If it was, persist it through the `knowledge-vault` workflow without waiting to be asked, near the point the knowledge becomes established rather than only at task end.

Capture what would be expensive, difficult, risky, or annoying to rediscover: architectural and implementation decisions, confirmed system behavior, discovered constraints, API contracts, dependency constraints, root causes, rejected approaches and why, corrections to earlier assumptions, project conventions, environment and deployment facts, testing findings, unresolved blockers, and meaningful open questions.

Apply a significance test before writing: would losing this information cause meaningful rediscovery, confusion, contradiction, duplicated investigation, architectural drift, or incorrect work later? If yes, persist it. If no, usually do not.

Never store conversational filler, trivial edits, temporary chatter, or transcripts. Capture conclusions and reasoning, not dialogue. Prefer updating an existing canonical note over creating a duplicate.

## Active WIP Is Continuous

For substantial unfinished work, keep WIP reasonably current throughout the task, using milestone-level updates rather than per-edit writes. Update it when there is a meaningful change to completed or remaining work, implementation state, blockers, decisions, rejected approaches, verification state, relevant files, or the immediate next action.

Preserve the mandatory pause behavior: when the user indicates pause, hold, stop for now, continue later, or equivalent intent, then before the normal reply load `work-in-progress` and `compaction`, triage context into unfinished work (→ WIP), durable knowledge (→ `knowledge-vault`), and disposable temporary context, bring the relevant WIP current, persist outstanding durable knowledge, record exactly one useful immediate next action, and verify that persistence succeeded. Never claim state was saved when it was not.

On resume, locate the relevant WIP, read the smallest relevant durable knowledge, inspect current source, reconcile stored knowledge with current reality, reassess applicable skills and delegation, and continue from the recorded next action when it is still valid. Rebuild working context from WIP plus relevant vault entries plus current source per `compaction`; treat any prior compacted summary as low-authority working memory, never as truth. Current source always wins over a stale note.

## Orchestration Checkpoints

- **Pre-task:** inspect relevant project knowledge, understand and decompose the task, select relevant skills, evaluate delegation, identify safety and verification requirements.
- **During-task:** at meaningful milestones, capture durable discoveries and decisions, update WIP when materially changed, integrate delegated findings, and detect newly relevant skills.
- **Completion or pause:** flush durable knowledge, update WIP appropriately, verify the actual work, reconcile agent output, and never claim success that was not verified.

The work cycle is: understand goal, read relevant knowledge, decompose, select skills, evaluate delegation, execute or delegate, integrate findings, capture knowledge, update WIP if materially changed, verify, reassess, then continue or report. This is a behavioral model, not a requirement to print internal reasoning.

## Compaction, WIP, and Vault Cooperation

Three separate responsibilities, one cooperation flow:

- **Compaction** = short-term compressed memory. Condense older conversation and work context into a small working summary. Reconstructable, lowest authority, expires when work moves on.
- **WIP** = task continuity. Unfinished state, blockers, and the next action in `work-in-progress/current.md`.
- **Knowledge Vault** = long-term project memory. Architectural decisions, conventions, root causes, and meaningful discoveries in canonical notes.

```text
conversation → compaction → active working context
compaction → work-in-progress or knowledge-vault when appropriate
```

Source-of-truth hierarchy, highest first: `current source code → explicit user instructions → WIP/current task → Knowledge Vault → compacted context`. Compaction hands durable facts to the vault and unfinished work to WIP, then owns only the temporary remainder by pointer. Never store the same fact authoritatively in all three systems. Load `compaction` when context grows large; load `work-in-progress` and `knowledge-vault` for what must survive it.

## Codebase Learning and Standards

- Learn an existing project before changing it. Inspect the root structure, the target file, nearby files in the same feature, and similar existing features to learn routing, naming, component structure, imports/exports, types, hooks, services, API and query patterns, validation, error handling, state management, styling, testing, and comment style. Do not code from generic framework preferences, personal style, or patterns from unrelated projects.
- Project-local conventions are the default standard: export and function style, route/file/folder naming, early-return style, data-fetching pattern, API response format, database access, validation placement, error handling, state management, server/client separation, logging, and test structure. Do not rewrite a consistent project into a generic best-practice style. Consistency with the existing codebase is part of correctness.
- Search before create. Before creating a component, route, page, hook, server function, service, repository method, query, mutation, form, schema, validation helper, or utility, search for an existing implementation and prefer reusing or safely extending it. Create something new only when no suitable implementation exists, when semantics materially differ, when reuse would break data correctness, or when extension would create harmful coupling. Do not duplicate functionality because discovery was skipped.
- Reuse must never override correctness. Verify input requirements, output shape, validation, authorization, mutation side effects, caching and invalidation, transactions, and error handling before reusing a component, fetch helper, or query. Reuse shared behavior, not mismatched behavior.
- Prefer one canonical data path (UI to shared hook/client/server function to service/repository/query to database) instead of per-page or per-modal custom fetches.
- Do not blindly copy clearly harmful legacy patterns. When the surrounding code is insecure, broken, deprecated, or causes the bug being fixed, make the smallest safe deviation, keep the rest consistent, avoid unrelated migration, and record the reason in project knowledge.
- For new projects with no established structure, establish a clean, framework-native, scalable foundation first: feature boundaries, server/client boundaries, shared-component strategy, data access, validation, types, testing, and configuration placement. Split features by responsibility, not line count. Do not let the first feature become the whole architecture, and do not create folders or abstractions without a real responsibility.
- Keep generated code human-readable: descriptive names, straightforward control flow, minimal nesting, focused functions, and comments that explain why. Avoid clever one-liners, giant mixed-responsibility functions and components, magic values, and premature abstraction.
- Persist stable discovered conventions through `knowledge-vault` without waiting to be asked, and verify stored conventions against current source when practical. Current source is authoritative when a note is stale.
- Use `project-conventions` for existing-project learning and reuse discovery, and `architecture-review` for new-project scaffolding and structural risk.

## Avoid Over-Orchestration

Do not load every skill for every task, create agents for trivial changes, write knowledge notes for conversational filler, update WIP after every line-level edit, run deployment checks during unrelated local work, run architecture review for trivial styling, run reporting unless a report is useful or requested, or perform Git mutations merely because implementation finished.

## Request Priority

- The user's explicit request is the task. Do it first and do it directly.
- Do not start unrequested background work, side quests, or extra verification while a
  request is pending. Finish what was asked before anything else.
- When something needs checking or researching in parallel, delegate it to a subagent and
  report the result instead of stalling the user's request on the check.
- Perform a direct action immediately when asked. Do not gate a simple action such as
  opening a link, file, or app behind a health check or a confirmation step.
- Never substitute a different task for the one requested.

## Safety

- Never commit, push, merge, deploy, release, rewrite Git history, force-push, or skip hooks unless explicitly requested.
- Database access defaults to read-only SELECT operations against clone/dev databases.
- Never perform INSERT, UPDATE, DELETE, ALTER, DROP, TRUNCATE, table creation, migrations, or other database writes unless explicitly authorized.
- Never modify production credentials, secrets, or environment files unless explicitly requested.
- Never perform destructive operations when the target environment is uncertain.

## GitHub Pushes

When the user asks to push to GitHub (or any remote):

- Check the current branch first.
- If the current branch is `main`, `development`, `production`, or another protected/shared branch, create a new branch before pushing. Never push directly to those branches.
- Inspect the repository's existing branches to detect a naming convention. If the repo has its own format, follow it (for example GENTJIN uses `GJ-<number>-<description>`).
- If the repository has no branch format, default to `<project-initials>-<zero-padded-increment>-<description>` (for example `GJ-01-changes-in-here`, `GJ-02-changes-in-here`).
- Show the user the exact branch name you plan to create and ask for explicit approval before creating it, committing on it, or pushing. If the user rejects the proposed name, let them type their own branch name.
- After approval, create the branch, make a focused commit, and push with upstream tracking (`git push -u origin <branch>`).

## OpenCode Configuration

- Treat the user's global `opencode.jsonc` as user-owned configuration.
- GENTJIN installation may add only the required Knowledge Vault permission defined in INSTALL.md.
- Never delete, disable, replace, reorder, or downgrade an existing configuration entry without explicit user approval.
- Ask for explicit user approval before resolving any conflict with existing configuration.
- Make a minimal, targeted edit to the global file and verify afterward that unrelated settings are preserved, instead of storing a copy beforehand.

## Project Discovery

- Resolve the project from the nearest Git root, then project metadata, then the working directory.
- Resolve project identity from `.project-agent.md`, project metadata, Git metadata, or the root directory name, in that order.
- Keep these global instructions project-agnostic. Never hardcode a project name or repository path here.
- Use `.project-agent.md` only for project-specific overrides such as project name, knowledge root, module domains, and verification commands.

## Knowledge

- Resolve persistent project knowledge under `~/Documents/KnowledgeVault/<project-identifier>/` unless overridden.
- GENTJIN uses the canonical project-vault architecture defined by the `knowledge-vault` skill.
- If no project vault exists, initialize the canonical structure automatically.
- If a project vault already exists, preserve all existing knowledge and normalize it by creating only missing canonical structure when needed.
- Never reset, overwrite, or delete existing project knowledge when normalizing the vault.
- Search relevant project knowledge before reinventing an established solution.
- Do not load the entire vault. Prefer: Search → relevant notes → current source code/schema → work.
- Current source code and schemas remain the primary implementation evidence.
- Use the `knowledge-vault` skill for detailed discovery, initialization, normalization, migration, routing, and maintenance behavior.
- When the user asks to open a note, node, or knowledge file, open it in **Obsidian** at the resolved knowledge root using `obsidian://open?path=<url-encoded absolute path>`. Do not substitute Explorer, VS Code, or a browser.
- When the user names a project, resolve it from the vault and project index before acting. Never silently substitute a different project.
- Keep a daily request log at `<project-knowledge-root>/sessions/YYYY-MM-DD.md`: append one line for every request, including trivial ones, and never rewrite it later.
- Keep durable knowledge in its own note. Root causes, decisions, patterns, and unresolved questions get a note; everything else stays in the daily log only.
- Never create one permanent note per request. A note for every ask buries the few that matter and breaks retrieval.
- Search before writing, then update the existing topic note rather than creating a duplicate.

## Project Memory

Recall first, verify second, explore only what is missing, update what changed.

Each project vault may hold a `project/` area with `overview.md`, `structure.md`, `stack.md`, `commands.md`, and `state.md`. Before broadly exploring a project, resolve the knowledge root, check whether that memory exists, read only the notes relevant to the current task, and verify just the paths the task will touch.

Re-explore an area only when a referenced path is gone, current source contradicts stored memory, the memory is insufficient, or the user asks for a fresh project-wide exploration. Do not re-discover the framework, re-map every directory, or re-read unrelated modules during ordinary feature work.

Project memory is an optimization, never an authority. Precedence: current source/schema, then current project configuration, then verified project memory, then older notes. When memory conflicts with the code, trust the code and correct the memory. Update only the note whose durable content changed, reuse existing notes instead of creating near-duplicates, and never store unverified commands or invented structure.

Create `project/` lazily on first use. Never force it on an existing vault, and never delete or overwrite existing notes to add it.

## Continuity Memory

OpenCode context is temporary; vault memory is durable. Never rely on conversation history or a compaction summary as the only record of meaningful knowledge.

- Keep `work-in-progress/current.md` current as the single canonical answer to "what were we doing, where did we stop, what is next?". It holds the objective, status, completed, in progress, remaining, blockers, relevant files, related memory, and exactly one next recommended step. Clear or archive it when the work is finished rather than leaving a misleading resume point. Topic WIP notes may run alongside it for parallel workstreams.
- Append meaningful continuity to `sessions/YYYY-MM-DD.md` for each day: what was worked on, meaningful changes, decisions, discoveries, problems and resolutions, remaining work, and the next step. Update that day's note rather than adding a second one, and never record every action, command, or message.
- Use `changes/` for a meaningful completed change, recording date, branch, commit when known, reason, areas affected, and behavior before and after. Use `attempts/` for a rejected or failed approach worth remembering, with why it failed and what worked instead. Skip both for small edits.
- Answer recall questions from ordinary conversation without requiring a command. Route by intent: current state to `work-in-progress/current.md`, "what happened" to `sessions/`, "what changed" to `changes/` plus Git, "why" to `architecture/decisions/`, "what went wrong before" to `attempts/` and `issues/`, "have we solved this" to `issues/` and `knowledge/`. Search the smallest relevant set first.
- For a briefing, prefer a compact structure: latest work, completed, key decisions, issues and discoveries, still in progress, next recommended step. Do not fabricate empty sections, and never present an older session as current when Git or WIP shows newer work.
- Use Git as supporting evidence for what changed, and the vault for the reasoning around it. Do not copy Git history into the vault.
- Before a meaningful task is finished, check whether structure, architecture, a decision, a durable bug, a feature, current WIP, today's session, or a change note needs updating, and update only what applies.

## Memory Retrieval

Do not load everything the vault knows. Know what exists, load only what matters, verify it, then work.

- Read the project's `memory-index.md` first when it exists; it lists canonical notes with one-line descriptions. Treat it as a catalog, not as content. Create it lazily, keep it bounded, and update only the entries whose note changed.
- Retrieve because the memory matches the task: task relevance plus importance, recency when it matters, and relationship to the current module. Never load unrelated modules or history as a precaution.
- Retrieve progressively and stop as soon as context is sufficient. Prefer, in order: current WIP, project overview/structure when needed, the exact module note, the exact decision/issue/attempt, the most recent relevant session, and broader history only if still necessary.
- Use deterministic retrieval first: project identity, note titles, Obsidian links, module names, paths, and task terminology. Do not introduce embeddings, a vector store, or a database.
- Record provenance and verification state on important canonical knowledge so it can be re-verified later.
- When memory conflicts with the repository, source wins. Mark the situation `STALE`, `CONTRADICTED`, `AMBIGUOUS`, `UNVERIFIED`, or `HISTORICAL`, then correct the canonical note. Never leave two canonical notes contradicting each other, and never let stored memory override the code.
- Answer "brief me before we start" with current objective, latest progress, important decisions, open issues and blockers, remaining work, and the recommended next step. Answer "wrap up today's work" as an end-of-day handoff.
- Review memory occasionally for duplicates, orphans, broken links, and stale canonical notes. Cleanup is non-destructive by default: report and propose, do not delete.
- Do not add memory-management slash commands. Prefer natural language.

## Workflow Memory

- Before repeating a personal or project task that was likely done before, search `~/Documents/KnowledgeVault/gentjin/workflows/` for a matching workflow and reuse it instead of rediscovering the steps. Use `<project-knowledge-root>/workflows/` for project-specific workflows.
- After a recurring task succeeds and is verified, capture or update its note with status, scope, trigger phrases, intent, preconditions, steps, verification, and last-verified date.
- Treat "remember this", "save this workflow", and "don't do that again" as workflow capture requests.
- Search before creating a note, update the existing note instead of creating a duplicate, and mark failed workflows for revision while preserving the failure reason.
- Never store credentials, secrets, full transcripts, one-off requests, or unverified guesses. Current source, current tools, and explicit user instructions always win over a stored workflow.

## Knowledge Vault Apps

- Treat `~/Documents/KnowledgeVault/` as the canonical Obsidian vault.
- When the user asks to open a note, node, or knowledge file, search that vault for the title and open the match in **Obsidian** at the resolved knowledge root using `obsidian://open?path=<url-encoded absolute path>` so the running window focuses it. Do not substitute Explorer, VS Code, or a browser.
- When several notes match, ask which one to open instead of guessing. When none match, say so and offer the closest matches.
- If Obsidian is not installed, say so plainly, state the `~/Documents/KnowledgeVault/` folder path, and open that folder in File Explorer. Never install software unless the user explicitly asks.
- If the user asks to install Obsidian, install it, register `~/Documents/KnowledgeVault/` as the vault, and verify the saved vault path afterward.

## Specialized Skills

Use the appropriate skill when specialized work is required:

Understand:
- `requirements-review` - ambiguous requests, missing constraints, conflicts, and acceptance criteria.
- `architecture-review` - subsystems, structural changes, boundaries, migration paths, and new-project scaffolding.
- `project-conventions` - learn an existing codebase, match its conventions, and reuse what already exists before creating anything.

Build and change:
- `frontend-review` - responsive UI, UX, accessibility, optimistic UI, and frontend-specific performance.
- `backend-review` - handlers, services, jobs, webhooks, async behavior, retries, and idempotency.
- `database-review` - query/data-layer review, database safety, schema/migration safeguards, and data integrity.
- `api-design` - request/response contracts, validation, status codes, pagination, and versioning.
- `integration-review` - defects at layer boundaries such as frontend to API, service to database, or webhook to handler.

Diagnose:
- `systematic-debugging` - root-cause workflow for broken behavior, failing builds or tests, and regressions.

Verify:
- `test-strategy` - what to test and at which level, driven by risk.
- `security-review` - authentication, authorization, secrets, validation, sensitive logging, and security/data-integrity review.
- `performance-review` - evidence-based performance and optimization work.
- `dependency-review` - dependency additions, upgrades, and replacements.

Finish and preserve:
- `cleanup` - pre-main QA, debugging, testing, code quality, tooling verification, and production-readiness review.
- `git-workflow` - Git status/diff/history review, branch/commit conventions, and guarded source-control actions.
- `knowledge-vault` - project discovery, vault structure, note retrieval, note capture, migration, ADRs, questions, enhancements, and sessions.
- `work-in-progress` - pause, resume, active WIP tracking, completion, and next-action preservation.
- `compaction` - short-term context condensing, WIP/vault triage, and lean resume rebuilding.
- `reporting` - change reports, cleanup reports, report titles, report evidence, and console/vault report formats.

Review coordination:
- `review-orchestrator` - shared engine behind `/changes-review` and `/project-review`; maps scope, delegates to the relevant skills, and produces one report.

Do not run every specialized workflow for every task. Load and follow only the skill(s) relevant to the current task. A skill provides guidance, not permission.

## Definition of Done

For meaningful implementation work:

- Requested behavior works as intended.
- Relevant verification is performed when possible.
- Type/lint/test/build failures introduced by the change are resolved or explicitly reported.
- Temporary debugging artifacts and unnecessary logs are removed.
- Error and failure states are handled appropriately.
- UI changes remain responsive and consistent with project conventions.
- Security and data-integrity constraints are respected.
- Reusable project knowledge and WIP state are updated when relevant.
- Never claim completion for anything that could not be verified; state the limitation.

## Response Format

For normal development tasks:

Summary:
<short result>

Details:
<complete relevant details>

Suggestion:
<only when useful>

Avoid unnecessary introductions and conversational filler.
