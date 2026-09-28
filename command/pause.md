---
description: Bring the active WIP current, flush outstanding durable knowledge, record one next action, and verify persistence before stopping work.
---

Perform a pause of the current work per the `work-in-progress` skill. This is the deterministic manual control for the pause behavior; the same thing must happen when the user says pause, hold, stop for now, continue later, or equivalent in natural language.

Treat "wrap up today's work", "give me an end-of-day handoff", and "save where we stopped" as the same workflow, and return a short handoff summary when one of those is used.

Do this before replying:

1. Load and follow `work-in-progress`. Load `compaction` for the triage step below.
2. Find the relevant WIP, or create one when there is meaningful unfinished work. If there is no meaningful unfinished work, say so and do not create artificial WIP.
3. Triage per `compaction` before persisting: separate unfinished work (→ WIP), durable knowledge (→ `knowledge-vault`), and temporary context (→ compacted remainder to discard). Never turn everything into permanent knowledge.
4. Bring it current: goal, current state, work completed, remaining work, important findings, decisions made, rejected approaches, files, blockers, and actual verification status.
5. Refresh `work-in-progress/current.md` so the canonical resume point is accurate, with exactly one `Next Recommended Step`.
6. Flush outstanding durable knowledge through `knowledge-vault` while it is still fresh, so WIP can reference it instead of reconstructing it. WIP plus the session note must be sufficient to resume even if any compacted summary is lost.
7. Record exactly one specific immediate `Next Action`.
8. Append a continuity entry to `sessions/YYYY-MM-DD.md` covering what was worked on, what changed, decisions, and what remains.
9. Preserve worthwhile unresolved investigations under `issues/questions/`, and noteworthy failed approaches under `attempts/`.
10. Verify the persistence actually succeeded by confirming the files exist and contain the state you claimed.

Never claim state was saved when persistence failed. Say what failed and what remains unrecorded.

Do not run unrelated cleanup, Git, deployment, database, or reporting workflows as part of a pause.
