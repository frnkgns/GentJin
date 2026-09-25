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

## OpenCode Configuration

- Treat the user's global `opencode.jsonc` as user-owned configuration.
- GENTJIN installation may add only the required Knowledge Vault permission defined in INSTALL.md.
- Never delete, disable, replace, reorder, or downgrade an existing configuration entry without explicit user approval.
- Ask for explicit user approval before resolving any conflict with existing configuration.
- Back up the global file before an approved change and verify that unrelated settings are preserved.

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
- When the user names a project, resolve it from the vault and project index before acting. Never silently substitute a different project.

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

- `knowledge-vault` - project discovery, vault structure, note retrieval, note capture, migration, ADRs, questions, enhancements, and sessions.
- `work-in-progress` - pause, resume, active WIP tracking, completion, and next-action preservation.
- `cleanup` - pre-main QA, debugging, testing, code quality, tooling verification, and production-readiness review.
- `frontend-review` - responsive UI, UX, accessibility, optimistic UI, frontend performance, and fallback review.
- `database-review` - query/data-layer review, database safety, schema/migration safeguards, and data integrity.
- `security-review` - authentication, authorization, secrets, validation, sensitive logging, and security/data-integrity review.
- `reporting` - change reports, cleanup reports, report titles, report evidence, and console/vault report formats.
- `git-workflow` - Git status/diff/history review, branch/commit conventions, and guarded source-control actions.

Do not run every specialized workflow for every task. Load and follow only the skill(s) relevant to the current task.

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
