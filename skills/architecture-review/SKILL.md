---
name: architecture-review
description: Analyze significant cross-module or process-level structural changes before implementing them. Use when adding a subsystem, redesigning a major flow, changing authentication architecture, introducing workers/queues/background services, integrating an external provider, creating shared infrastructure, changing module boundaries or communication, scaffolding a new project or its first feature, or when the user asks how something should be built. Not for matching or reusing existing code patterns within a known structure; use `project-conventions` for that.
---
# Architecture Review

## Principle

Prefer the smallest architecture change that solves the real problem. Do not introduce unnecessary abstraction.

## Inspect

- Current architecture and module boundaries.
- Affected modules and their responsibilities.
- Ownership boundaries: who owns a responsibility, and where it must not leak.
- Data flow across the change, including persistence and external calls.
- Dependency direction; flag cycles and inverted dependencies.
- Failure boundaries: what happens when each dependency fails, times out, or returns partial results.
- Lifecycle and process ownership, especially for long-running work such as workers, queues, schedulers, and background services.
- Compatibility with existing conventions, patterns, and naming.

## Evaluate

- Alternatives with their tradeoffs, including doing nothing.
- Migration path from the current structure, and whether it can be incremental.
- Rollback and coexistence during rollout.
- Operational cost: observability, failure modes, and new failure surfaces.

## Output

Present the recommended structure, why it fits this codebase, the rejected alternatives with reasons, the migration steps, and the risks with mitigations. Then ask for approval before implementing a structural change.

## Rules

- Do not redesign working code that the request does not touch.
- Do not add layers, interfaces, or abstractions without a demonstrated need.
- Keep responsibilities where the project already keeps them.
- Do not implement structural changes without explicit approval.

## New Projects and Scaffolding

Apply this when the project is new or has no meaningful established structure. The first feature must not accidentally become the architecture for the whole project.

- Prefer framework-native routing, server/client boundaries, configuration locations, and data-loading patterns. Do not fight the framework to impose a generic folder structure.
- Separate responsibilities the project will actually have: routes or pages, reusable UI, feature-specific components, hooks or composables, services, server logic, data access, schemas and validation, types, utilities, configuration, tests. Every directory needs a real responsibility; do not create folders to look sophisticated.
- For medium or growing applications, prefer feature-oriented organization such as `features/<name>/{components,hooks,services,schemas,types}`, with genuinely shared pieces under `components/shared` and cross-cutting code under `lib` or `utils`. This is an example, not a universal requirement; adapt to the framework, runtime, scale, deployment model, and team.
- Split features by responsibility, not by line count. A large file with one clear responsibility can stay; a file mixing unrelated responsibilities should be split. UI components should not own rendering, data fetching, mutation logic, validation, transformations, and business rules at once. Server files should not combine routing, validation, authorization, queries, business rules, and third-party calls in one handler.
- Extract reusable components only when a pattern genuinely repeats, reuse is realistically expected, and the extracted unit has a clear independent responsibility. Do not create components to increase component count or move everything into a global shared directory prematurely.
- Design for likely change: keep external integrations behind focused interfaces, do not duplicate business rules across UI and backend, centralize shared validation when appropriate, reuse domain types carefully, isolate provider-specific logic, and keep configuration separate from behavior.
- Do not over-engineer for hypothetical requirements. Scalability means making expected change manageable, not predicting every future feature.

## Proportional Design

Prefer the maintainable separated implementation over a quick monolith when the feature is substantial, and do not turn a small feature into an excessive architecture exercise. Apply `project-conventions` for convention matching and reuse discovery.
