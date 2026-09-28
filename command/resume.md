---
description: Reload the active WIP and relevant knowledge, verify them against current source, and continue from the recorded next action.
---

Resume previously paused work per the `work-in-progress` skill. This is the deterministic manual control for the resume behavior; the same thing must happen when the user says continue, resume, or revisit in natural language.

Do this before acting:

1. Load and follow `work-in-progress`. Load `compaction` for rebuild discipline.
2. Locate the relevant active WIP. Read `work-in-progress/current.md` when it exists, since it is the canonical resume point. If several workstreams match, ask which one instead of guessing.
3. Read only the necessary context: goal, current state, completed and remaining work, decisions, rejected approaches, blockers, verification status, the latest relevant `sessions/` entry, related knowledge, and the next action.
4. Inspect current source code and reconcile it with the stored state. Current source wins whenever they disagree, and correct the stale note rather than propagating the contradiction.
5. Rebuild working context from WIP plus relevant Knowledge Vault entries plus current source per `compaction`. Treat any prior compacted summary as a low-authority hint: re-verify its assumptions and next action before acting, never trust it blindly.
6. Reassess applicable skills and delegation now that the work is active again.
7. Continue from the recorded next action when it is still valid; if it is not, say why and state the correct next action.
8. Refresh `work-in-progress/current.md` as progress is made.

Do not restart earlier investigation that the WIP and knowledge already settle, and do not ask the user to restate decisions that are already recorded.
