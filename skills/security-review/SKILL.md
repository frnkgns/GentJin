---
name: security-review
description: Review changed code for application security and data-integrity risks. Use for authentication, authorization, user-owned records, external input, secrets/tokens, sensitive logs, mutations, payments, permissions, downloaded/untrusted code, or cleanup of security-sensitive changes.
---
# Security Review

Review only the relevant changed code and directly related trust boundaries.

## Authentication and Authorization
- Authentication must be enforced where required.
- Authorization must be enforced server-side, not trusted solely to the client.
- Verify users cannot access another user's records by changing IDs or request parameters.
- Verify ownership/role/permission constraints on reads and mutations.

## Input and Mutation Safety
- Validate untrusted input at appropriate boundaries.
- Prevent duplicate/inconsistent mutation outcomes.
- Treat payment/security-sensitive operations conservatively; do not use optimistic success when authoritative confirmation is required.
- Do not weaken existing safeguards to make a flow appear functional.

## Secrets and Logging
- Never expose secrets, credentials, tokens, environment values, private database contents, or sensitive user data to client code or logs.
- Remove temporary debug logs.
- Use proper production logging; genuine errors may use `console.error` when no logger exists.
- Ensure error messages do not leak sensitive implementation details.

## Dependencies and Untrusted Code
For downloaded or unfamiliar code, inspect install/build scripts, lifecycle hooks, network/process/file-system behavior, credential access, obfuscated/minified payloads, suspicious binaries, and dependency changes before execution. Do not execute suspicious code merely to determine what it does.

## Reporting
State concrete findings and evidence. Distinguish confirmed issues from risks requiring further verification. Do not claim a security property was verified if the necessary environment/test could not be run.
