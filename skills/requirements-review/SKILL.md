---
name: requirements-review
description: Clarify ambiguous requests, missing constraints, conflicting rules, and acceptance criteria before implementation. Use when a feature request is vague, business rules are incomplete, more than one interpretation is genuinely plausible, or implementation would require guessing a decision that changes the result. Not for ordinary requests that are already clear enough to build.
---
# Requirements Review

## Principle

Resolve only what materially affects implementation. Do not block clear, simple requests with unnecessary questions.

## Extract

- Requested behavior, in plain terms.
- Actors and roles involved, including anonymous and administrative users.
- Allowed states and prohibited states.
- Edge cases, limits, and error conditions.
- Behavior for existing records and existing data.
- Backward compatibility and migration impact.
- Acceptance criteria that can be verified.
- Explicit non-goals.

## Output

Produce a short confirmation containing:

1. The understood requirement in one or two sentences.
2. Assumptions that materially shape the implementation.
3. Open questions that would change the implementation, with the options and their consequences.
4. Proposed acceptance criteria.
5. Impact on existing workflows, data, and integrations.

## Rules

- Ask only questions whose answers change the design, data model, or behavior. Resolve trivia from the existing code and conventions.
- Prefer evidence from the current source, schema, and existing knowledge over assumptions.
- When a question is blocking, ask it before writing code, not after.
- When the request is clear enough to act on, state the interpretation and proceed rather than interrogating the user.
- Do not silently change scope, add features, or redesign unrelated behavior.
- Record decisions that are likely to be reused in the project's knowledge structure.

## Reporting

State the confirmed requirement, the assumptions made, the questions that remain, and where each decision will be verified. Flag any interpretation the user has not confirmed.
