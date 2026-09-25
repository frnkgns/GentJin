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
| Work disappears between sessions | WIP state and persistent knowledge |
| “It looked done” | Cleanup, QA, and explicit verification |
| Reviews happen too late | Frontend, database, and security reviews |
| Risky defaults | Read-only data access and guarded Git actions |
| Reports take too long | Consistent, evidence-based reporting |

> **The goal:** make good engineering behavior easier to repeat, not harder to remember.

## Workflow at a glance

```text
        A clear request
              │
              ▼
       Discover the context
       project · code · notes
              │
              ▼
         Plan and build
              │
              ▼
      Focused review when needed
   UI · data · security · Git
              │
              ▼
       Verify and clean up
     tests · lint · build · QA
              │
              ▼
       Report and preserve
     WIP · reports · knowledge
```

GENTJIN supports the full development loop without forcing every workflow into
every conversation.

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

OpenCode will detect your operating system and configuration paths, create a
permanent GENTJIN home at `<user-home>/.config/opencode/GentJin/`, preserve
your existing setup, back up conflicts, and verify the installed files. Restart
OpenCode when it finishes.

For path detection, conflict handling, updates, and troubleshooting, see
[INSTALL.md](INSTALL.md).

## Commands

| Command | Use it when you want to... |
| --- | --- |
| `/cleanup` | Finish current work with QA, cleanup, and a final report. |
| `/report` | Turn the current work into a clear, evidence-based report. |
| `/status` | See the branch, pending changes, active WIP, and open questions. |
| `/task` | Handle a general-purpose software or laptop task, with reusable workflow memory and `vscode` and `dev` branches. |

Commands are intentionally short entry points into larger, repeatable
workflows.

## Skills

Each skill is a focused playbook. GENTJIN loads the relevant guidance instead
of asking the agent to remember every rule at once.

| Skill | Focus |
| --- | --- |
| `cleanup` | Pre-merge QA, debugging, cleanup, and production readiness. |
| `frontend-review` | Responsive UI, accessibility, UX states, and performance. |
| `database-review` | Queries, data layers, performance, failures, and migration safety. |
| `security-review` | Authentication, authorization, secrets, input, and data integrity. |
| `git-workflow` | Guarded branches, commits, pull requests, and source control. |
| `work-in-progress` | Pause, resume, and preserve unfinished implementation work. |
| `knowledge-vault` | Project discovery and persistent knowledge in Obsidian. |
| `reporting` | Concise console reports and durable vault documentation. |

## Built for the whole lifecycle

### Before the work

- Discover the project and its existing knowledge.
- Check active WIP, open questions, and recent decisions.
- Start from the current source code rather than assumptions.

### During the work

- Keep the active context small and relevant.
- Use the right review skill at the right trust boundary.
- Preserve decisions, rejected approaches, and meaningful discoveries.

### Before handoff

- Review the diff and affected user flows.
- Check loading, error, empty, nullable, and failure states.
- Run the project's real test, lint, typecheck, and build commands.
- Remove temporary artifacts and unnecessary logs.

### After the work

- Write an evidence-based report.
- Capture reusable knowledge where it belongs.
- Keep unfinished work visible with one clear next action.

## Safe by default

GENTJIN is designed to make the cautious path the easy path:

- Database access is read-only unless write access is explicitly authorized.
- Commits, pushes, merges, and history changes require an explicit request.
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
│   ├── cleanup.md
│   ├── report.md
│   ├── status.md
│   └── task.md
├── skills/                   Focused, trigger-based workflows
│   ├── cleanup/
│   ├── database-review/
│   ├── frontend-review/
│   ├── git-workflow/
│   ├── knowledge-vault/
│   ├── reporting/
│   ├── security-review/
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
