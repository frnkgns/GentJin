---
name: performance-review
description: Find real performance problems using measurement instead of speculation. Use when performance is reported as an issue, the user asks for optimization, a review spots potentially expensive behavior, or a change is suspected of causing a measurable slowdown. Owns cross-stack methodology for caching, N+1 behavior, and resource leaks; use `frontend-review` for render performance and `database-review` for query cost.
---
# Performance Review

## Principle

Measure first. Do not add caching or memoization without evidence that it is useful and safe.

## Workflow

1. Measure or gather evidence: timings, query counts, bundle sizes, request counts, profiles, or traces from the actual runtime.
2. Locate the bottleneck, not the suspected code.
3. Explain the cause in terms of the measured cost.
4. Suggest the smallest useful optimization with its expected effect.
5. Implement only when authorized.
6. Measure again and report the before/after result.

## Review Areas

- Unnecessary or duplicated network requests.
- N+1 query and request behavior.
- Repeated server work that could be done once per request or per batch.
- Expensive rendering, unnecessary re-renders, and unstable effect dependencies.
- Over-fetching data or columns, and unbounded result sets.
- Large client bundles, oversized client components, and heavy dependencies.
- Blocking tasks on critical paths, and sequential awaits that could run concurrently.
- Inefficient loops and repeated work inside render or request paths.
- Caching opportunities, including correct invalidation.
- Memory and resource leaks: subscriptions, listeners, timers, connections, and unbounded collections.

## Rules

- Do not recommend an optimization without a plausible measurable win.
- Do not trade correctness for speed; caching stale or shared mutable data is a defect.
- Respect existing caching layers and conventions; do not add a new cache layer casually.
- Leave frontend-specific rendering and UX performance checks to `frontend-review` and query-level cost to `database-review`; this skill covers cross-stack performance methodology.
- Report the measurement used for every performance claim, or label it as an unverified hypothesis.
