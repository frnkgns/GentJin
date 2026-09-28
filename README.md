<div align="center">

# GENTJIN

### A modular development system for [OpenCode](https://opencode.ai/docs/)

**Less prompt noise. More deliberate engineering.**

GENTJIN gives OpenCode a focused operating system for planning, building,
reviewing, and learning from real software work.

[Explore the workflow](#workflow-at-a-glance) · [Browse the skills](#skills)

</div>

---

## The problem

Most agent setups solve one problem at a time: a giant instruction file, a
forgotten cleanup step, or a review that only happens when someone remembers.

That approach creates noise. Important rules compete with one another, context
gets lost between sessions, and the final review becomes an afterthought.

## The GENTJIN approach

GENTJIN keeps the always-loaded rules small and moves specialized workflows
into focused skills that are activated when they are relevant.

| Everyday problem | GENTJIN response |
| --- | --- |
| One oversized instruction file | Modular skills with clear triggers |
| Skills that only run when remembered | Semantic capability selection, not keyword matching |
| Work disappears between sessions | Continuous knowledge capture and active WIP |
| Long multi-part tasks | Bounded, skill-aware agent delegation |
| “It looked done” | Cleanup, QA, and explicit verification |
| Reviews happen too late | Read-only `/changes-review` and `/project-review` |
| Risky defaults | Read-only data access and guarded Git actions |
| Reports take too long | Consistent, evidence-based reporting |

> **The goal:** make good engineering behavior easier to repeat, not harder to remember.

## Orchestration

GENTJIN behaves as an orchestration layer rather than a folder of optional
instructions. For meaningful work it:

- inspects and learns an existing project before changing it, so new code
  matches the conventions already in the repository instead of a generic style;
- searches for an existing implementation before creating a component, route,
  query, or helper, and reuses it unless that would break correctness;
- infers which skills apply from the task itself, instead of waiting for a
  keyword or a named skill;
- delegates independent, substantial workstreams to agents when that genuinely
  helps, while the main agent stays the orchestrator;
- captures durable project knowledge as decisions become established, gated by
  a significance test so notes never become transcripts;
- keeps WIP current during substantial unfinished work, and pauses and resumes
  on plain language such as "hold on" or "let's continue";
- reassesses skills, agents, and knowledge as the task evolves instead of
  locking in the first routing decision;
- verifies before claiming success, and scales orchestration up with complexity
  and risk rather than running everything by default.

For a new project with no established structure, the same layer establishes a
framework-native, feature-oriented foundation first, so the first feature does
not become the architecture for everything else.

Explicit commands remain available for deterministic control. They are a manual
override, not the only way the workflow runs.

## Project memory

GENTJIN keeps a small, durable understanding of each project so a later session
does not rediscover what an earlier one already learned. Each project vault gets
a `project/` area:

```text
project/
├── overview.md    what this repository is
├── structure.md   layout, directory responsibilities, relationships
├── stack.md       verified runtime, frameworks, data layer, tooling
├── commands.md    verified install, dev, build, test, lint, type-check
└── state.md       identity, known roots, last verified revision
```

The behavior it enables:

```text
Recall first.
Verify second.
Explore only what is missing.
Update what changed.
```

Project memory is an optimization, never an authority. Current source code and
schema always win; when stored memory contradicts the repository, the code is
believed and the memory is corrected. Each note carries a `Last verified`
marker as a staleness hint, and those markers never trigger an automatic full
rescan. Ordinary feature work reads only the notes it needs and updates only
the note whose durable content changed.

Existing vaults keep working. The `project/` area is created lazily, on first
use, and no existing note is ever deleted or overwritten to add it. Project
memory is not a copy of the repository, not a replacement for verifying code,
not a conversation log, and not a reason to scan the whole project each time.

## Continuity memory

Context is temporary; the vault is durable. GENTJIN keeps enough state to answer
ordinary questions about previous work without relying on the conversation:

```text
work-in-progress/current.md   where we stopped and what is next
sessions/YYYY-MM-DD.md        what happened on a given day
changes/                      a meaningful completed change, with reason
attempts/                     a rejected approach, why it failed, what worked
architecture/decisions/       why an important choice was made
```

That means these work in plain conversation, with no command:

```text
"Brief me."                    "Where did we stop?"
"What did we do yesterday?"     "What is still unfinished?"
"Continue what we were doing."  "Why did we do it this way?"
"Didn't we try this before?"   "What changed this week?"
```

A briefing stays compact — latest work, completed, key decisions, issues and
discoveries, still in progress, next recommended step — and never presents an
older session as current when Git or WIP shows newer work. Git is evidence for
what changed; the notes hold the reasoning.

Three systems cooperate without duplicating each other:

```text
conversation → compaction → active working context
compaction → work-in-progress or knowledge-vault when appropriate
```

- **Compaction** = short-term compressed memory. Reconstructable working recall,
  lowest authority, expires when work moves on.
- **WIP** = task continuity. Unfinished state, blockers, and the next action.
- **Knowledge Vault** = long-term project memory. Decisions, conventions, root
  causes, and meaningful discoveries.

Authority runs `current source code → user instructions → WIP → Knowledge
Vault → compacted context`. Compaction hands durable facts to the vault and
unfinished work to WIP, then keeps only the temporary remainder by pointer.
`/pause` separates state into those three buckets; `/resume` rebuilds from
WIP plus relevant vault notes plus current source rather than trusting an old
summary. A compaction or a closed session therefore does not lose the
engineering state.

## Memory retrieval

Each vault keeps a small `memory-index.md` catalog: one line per topic, linking
to the canonical note with a one-line description. It answers "what knowledge
exists, and where is it?" without opening a single note, and never grows into a
second copy of the documentation.

Retrieval is relevance-gated and progressive. A request about one module does
not pull in unrelated modules or old sessions. Memory loads in budgeted order —
current WIP, project overview, the exact module note, the exact decision or
issue, the most recent relevant session — and stops as soon as context is
sufficient. Lookup is deterministic first: note titles, links, module names,
paths, and task terminology. No embeddings, no vector store, no database.

Important canonical notes record where they came from and when they were last
verified. When memory disagrees with the repository, the code wins, the
situation is marked `STALE`, `CONTRADICTED`, `AMBIGUOUS`, `UNVERIFIED`, or
`HISTORICAL`, and the note is corrected. Two canonical notes never end up
contradicting each other.

"GentJin should become better at remembering without becoming heavier to use."

## Workflow at a glance

```text
        A clear request
              │
              ▼
     Understand the change
 requirements · architecture
              │
              ▼
          Plan and build
              │
              ▼
        /changes-review
   when the implementation
      needs a review
              │
              ▼
         /cleanup
  when the work is finished
              │
              ▼
     /deployment-check
   before production
              │
              ▼
        /git-push
   when you ask for it
```

`/project-review` is the optional deeper audit of an entire codebase, not
something expected on every feature. GENTJIN supports the full development loop
without forcing every workflow into every conversation.

## Quick start

Clone the repository, open it in OpenCode, and let the agent install GENTJIN
for you:

```bash
git clone https://github.com/frnkgns/GentJin.git
cd GentJin
opencode
```

Then enter this command in OpenCode:

```text
Install GENTJIN by following INSTALL.md
```

GENTJIN installs directly into OpenCode's existing runtime directories:

- `<user-home>/.config/opencode/AGENTS.md`
- `<user-home>/.config/opencode/command/`
- `<user-home>/.config/opencode/skills/`

GENTJIN keeps only lightweight installation metadata under:

```text
<user-home>/.config/opencode/.gentjin/
```

```text
No duplicate GENTJIN source tree is retained after installation.
```

OpenCode detects your operating system and configuration paths, installs the
runtime files in place, preserves your existing setup, adds only the required
Knowledge Vault permission to the global configuration, and, with explicit
approval, renames a legacy `Documents/ObsidianVault` folder to
`Documents/KnowledgeVault` so Obsidian keeps pointing at the same vault. It
keeps no copies of anything and verifies the result. Restart OpenCode when it
finishes.

For path detection, conflict handling, updates, and troubleshooting, see
[INSTALL.md](INSTALL.md).

The cloned repository is not required after a successful installation and can be
deleted.

Run `/update-gentjin` for normal updates. It uses `<user-home>/.config/opencode/.gentjin/manifest.json` to refresh only GENTJIN-owned files, asks before replacing anything you modified yourself, removes stale GENTJIN-owned files the repository no longer contains, preserves your own skills and commands, retires an old `<user-home>/.config/opencode/GentJin/` folder if one is present, and re-checks the Knowledge Vault permission. It retrieves the current source from an existing clone when one is available, otherwise from a temporary checkout that it removes afterwards. No backup copies are kept.

## Commands

| Command | Use it when you want to... |
| --- | --- |
| `/install-gentjin` | Install GENTJIN into your global OpenCode configuration. |
| `/update-gentjin` | Update the installed GENTJIN copy from a newer repository state. |
| `/cleanup` | Finish current work with QA, cleanup, and a final report. |
| `/report` | Turn the current work into a clear, evidence-based report. |
| `/status` | See the branch, pending changes, active WIP, and open questions. |
| `/task` | Handle a general-purpose software or laptop task, with reusable workflow memory and `vscode` and `dev` branches. |
| `/deployment-check` | Analyze deployment readiness, report blockers and warnings, and suggest next steps without modifying the project until you approve. |
| `/changes-review` | Review the current changes and their impact radius without modifying the project until you approve fixes. |
| `/project-review` | After confirmation, perform a comprehensive read-only review of the entire relevant project and suggest improvements before any changes are made. |
| `/pause` | Bring the active WIP current, flush durable knowledge, and record one next action. |
| `/resume` | Reload the WIP and relevant knowledge, verify them against current source, and continue. |
| `/git-push` | Commit and push the current changes on a new branch, asking for approval first. |

Commands are intentionally short entry points into larger, repeatable
workflows.

## Skills

Each skill is a focused playbook. GENTJIN loads the relevant guidance instead
of asking the agent to remember every rule at once.

| Skill | Focus |
| --- | --- |
| `requirements-review` | Ambiguity, constraints, conflicts, and acceptance criteria before implementation. |
| `architecture-review` | Cross-module structural changes, boundaries, data flow, and new-project scaffolding. |
| `project-conventions` | Learn an existing codebase, follow its conventions, and reuse what already exists. |
| `refactoring-migration` | Staged execution of large refactors, data or contract migrations, dual-write, and flag lifecycle. |
| `frontend-review` | Responsive UI, accessibility, UX states, and frontend performance. |
| `backend-review` | Handlers, services, jobs, webhooks, async behavior, retries, and idempotency. |
| `database-review` | Queries, data layers, performance, failures, and migration safety. |
| `api-design` | Request/response contracts, validation, pagination, and versioning. |
| `integration-review` | Defects at the seams between layers and systems. |
| `systematic-debugging` | Root-cause workflow for broken behavior and regressions. |
| `incident-response` | Live production impact: severity, mitigation, rollback, timeline, and hotfix discipline. |
| `test-strategy` | Risk-driven decisions about what to test and at which level. |
| `security-review` | Authentication, authorization, secrets, input, and data integrity. |
| `performance-review` | Evidence-based measurement, bottlenecks, and safe optimization. |
| `dependency-review` | Dependency additions, upgrades, and replacement risk. |
| `deployment-readiness` | Build and CI, environment parity, migration safety, rollback, and secret exposure. |
| `review-orchestrator` | Shared engine behind the review commands; delegates to the skills above. |
| `cleanup` | Pre-merge QA, debugging, cleanup, and production readiness. |
| `git-workflow` | Guarded branches, commits, pull requests, and source control. |
| `work-in-progress` | Pause, resume, and preserve unfinished implementation work. |
| `compaction` | Condense older context into a lean working summary; route WIP and durable knowledge outward. |
| `knowledge-vault` | Project discovery and persistent knowledge in the Knowledge Vault. |
| `reporting` | Concise console reports and durable vault documentation. |

## Built for the whole lifecycle

### Before the work

- Discover the project and its existing knowledge.
- Check active WIP, open questions, and recent decisions.
- Read the project's `project/` memory first, then verify only the paths the
  task will touch.
- Start from the current source code rather than assumptions.
- Learn the existing conventions and search for an existing implementation
  before creating anything new.
- Use `requirements-review` and `architecture-review` when the request is
  ambiguous or the change is structural.

### During the work

- Keep the active context small and relevant.
- Use the right review skill at the right trust boundary.
- Preserve decisions, rejected approaches, and meaningful discoveries.

### Before handoff

- Review the diff and affected user flows.
- Check loading, error, empty, nullable, and failure states.
- Run the project's real test, lint, typecheck, and build commands.
- Remove temporary artifacts and unnecessary logs.
- Run `/changes-review` when the work needs a read-only second opinion before
  fixes.

### After the work

- Write an evidence-based report.
- Capture reusable knowledge where it belongs.
- Keep unfinished work visible with one clear next action.

## Safe by default

GENTJIN is designed to make the cautious path the easy path:

- Database access is read-only unless write access is explicitly authorized.
- Commits, pushes, merges, and history changes require an explicit request.
- Global OpenCode configuration is additive: GENTJIN adds only its required
  Knowledge Vault permission and never removes existing entries.
- The legacy `Documents/ObsidianVault` folder is renamed to
  `Documents/KnowledgeVault` only with explicit approval, and Obsidian's saved
  path is updated to follow it.
- Credentials and environment values stay out of the repository.
- Verification is reported honestly; checks are never claimed without evidence.
- Real application data is preferred over mock or placeholder data.
- Security and data-integrity checks are applied when the change warrants them.

## Repository map

```text
.
├── AGENTS.md                 Global behavior and project-agnostic rules
├── INSTALL.md                Agent-assisted installation runbook
├── command/                  Reusable slash commands
│   ├── changes-review.md
│   ├── cleanup.md
│   ├── deployment-check.md
│   ├── git-push.md
│   ├── install-gentjin.md
│   ├── pause.md
│   ├── project-review.md
│   ├── report.md
│   ├── resume.md
│   ├── status.md
│   ├── task.md
│   └── update-gentjin.md
├── skills/                   Focused, trigger-based workflows
│   ├── api-design/
│   ├── architecture-review/
│   ├── backend-review/
│   ├── cleanup/
│   ├── compaction/
│   ├── database-review/
│   ├── dependency-review/
│   ├── deployment-readiness/
│   ├── frontend-review/
│   ├── git-workflow/
│   ├── incident-response/
│   ├── integration-review/
│   ├── knowledge-vault/
│   ├── performance-review/
│   ├── project-conventions/
│   ├── refactoring-migration/
│   ├── reporting/
│   ├── requirements-review/
│   ├── review-orchestrator/
│   ├── security-review/
│   ├── systematic-debugging/
│   ├── test-strategy/
│   └── work-in-progress/
└── opencode.jsonc            Local OpenCode configuration (not installed)
```

## Contributing

Improvements are welcome when they make the system clearer, safer, or more
useful:

- Keep skills focused on one recognizable trigger or outcome.
- Write descriptions that explain both **what** the skill does and **when** it
  should be used.
- Avoid secrets, machine-specific paths, and generated artifacts.
- Preserve the project's existing conventions before adding new ones.

Open an issue or pull request with a focused proposal and a short explanation
of the problem it solves.

<div align="center">

**Build with context. Review with rigor. Remember what matters.**

</div>
