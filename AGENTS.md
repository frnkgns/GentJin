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

## Safety

- Never commit, push, merge, deploy, release, rewrite Git history, force-push, or skip hooks unless explicitly requested.
- Database access defaults to read-only SELECT operations against clone/dev databases.
- Never perform INSERT, UPDATE, DELETE, ALTER, DROP, TRUNCATE, table creation, migrations, or other database writes unless explicitly authorized.
- Never modify production credentials, secrets, or environment files unless explicitly requested.
- Never perform destructive operations when the target environment is uncertain.

## Project Discovery

- Resolve the project from the nearest Git root, then project metadata, then the working directory.
- Resolve project identity from `.project-agent.md`, project metadata, Git metadata, or the root directory name, in that order.
- Keep these global instructions project-agnostic. Never hardcode a project name or repository path here.
- Use `.project-agent.md` only for project-specific overrides such as project name, knowledge root, module domains, and verification commands.

## Knowledge

- Resolve persistent project knowledge under `~/Documents/ObsidianVault/<project-identifier>/` unless overridden.
- GENTJIN uses the canonical project-vault architecture defined by the `knowledge-vault` skill.
- If no project vault exists, initialize the canonical structure automatically.
- If a project vault already exists, preserve all existing knowledge and normalize it by creating only missing canonical structure when needed.
- Never reset, overwrite, or delete existing project knowledge when normalizing the vault.
- Search relevant project knowledge before reinventing an established solution.
- Do not load the entire vault. Prefer: Search → relevant notes → current source code/schema → work.
- Current source code and schemas remain the primary implementation evidence.
- Use the `knowledge-vault` skill for detailed discovery, initialization, normalization, migration, routing, and maintenance behavior.

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
