---
name: compaction
description: Condense older conversation and work context into a small, accurate working summary when context grows large. MUST be used when repeated discussion, obsolete reasoning, explored-and-discarded paths, or topic drift obscure the current goal, before a pause, or when rebuilding context after interruption. Cooperates with work-in-progress for unfinished task state and knowledge-vault for durable knowledge; never replaces either or stores permanent truth.
---
# Compaction

Short-term context management. It turns a long conversation into a small,
accurate working summary so work can continue without re-reading everything.

It is reconstructable working memory, not permanent truth.

## What It Is and Is Not

| System | Job | Lives in |
| --- | --- | --- |
| `compaction` | Compressed short-term memory: what matters right now to continue | Conversation / working summary |
| `work-in-progress` | Task continuity: unfinished state, blockers, next action | `work-in-progress/current.md` plus topic WIP notes |
| `knowledge-vault` | Durable long-term memory: decisions, conventions, root causes, discoveries | Canonical vault notes |

Cooperation flow:

```text
conversation → compaction → active working context
```

Important findings discovered along the way flow outward only when they qualify:

```text
compaction → work-in-progress  (unfinished work, blockers, next action)
compaction → knowledge-vault   (durable decisions, conventions, root causes)
```

Never turn everything into permanent knowledge. Most compacted detail is
temporary and is discarded once it no longer affects the task.

## Triggers

OpenCode does not expose an exact context-window size, so triggers are
semantic, not token counts. Compact when any of these hold:

- Repeated discussion of the same point across many turns.
- Obsolete intermediate reasoning piles up (tried paths, superseded plans).
- Exploration covered several files or approaches but only the outcome matters.
- Topic drift: the active goal is buried under earlier unrelated work.
- Re-reading the same files or notes because the thread is too long to scan.
- A milestone completed and the steps behind it no longer affect what is next.
- Before `/pause`, so pause can separate durable state from disposable context.
- After interruption, resume, or delegation return, to rebuild a lean context.
- The agent must summarize "where we are" before continuing usefully.

Do not compact trivia, single-question tasks, or a session with no meaningful
accumulation. Do not compact merely because a message count was reached when
the thread is still focused and small.

## Preserve

A compacted summary keeps only what is needed to continue correctly:

- Current goal and explicit requirements and constraints.
- Important user decisions, including overrides of earlier stored knowledge.
- Files and components currently being worked on, with paths.
- Completed work that affects what remains.
- Unresolved problems, blockers, and open questions.
- Exactly one next action or a small ordered set of next actions.
- Relevant technical facts: confirmed behavior, versions, commands, schema
  shapes, API contracts, error messages that still matter.

Keep it dense. One line per fact beats one paragraph per fact.

## Remove

Drop what no longer affects the task:

- Repeated discussion restated in different words.
- Obsolete intermediate reasoning and superseded plans.
- Irrelevant exploration: files read and discarded, paths that led nowhere.
- Duplicated information already recorded in WIP or the vault; reference it.
- Temporary details: transient file contents, tool chatter, one-off command
  output, conversational filler.
- Old hypotheses disproven by current source code.

When in doubt, keep the conclusion and drop the transcript.

## Evidence Labels

Clearly separate these inside every compacted summary:

- `Verified`: checked against current source, schema, config, or a real run.
- `Decided`: explicit user choice or accepted instruction for this task.
- `Assumed`: working hypothesis not yet confirmed; needs verification.
- `Open`: unresolved question or blocker; owned by WIP or `issues/questions/`.

Never present an assumption as verified. Never present a compacted summary as
verified truth at all: it is a working recall aid until re-checked.

## Authority Hierarchy

When sources disagree, believe them in this order:

```text
current source code / schema / configuration
      ↓
explicit current user instructions
      ↓
work-in-progress/current task state
      ↓
Knowledge Vault canonical memory
      ↓
compacted context (lowest authority)
```

A compacted summary never overrides source code, a current user instruction,
active WIP, or canonical vault knowledge. On conflict, trust the higher source
and correct the summary. Record provenance (file paths, branch, commit) on any
fact that will need re-verification.

## Procedure

### 1. Triage before summarizing

Sort every candidate fact into exactly one bucket:

1. `Unfinished work` → synchronize with `work-in-progress` per that skill.
   Update `work-in-progress/current.md`: objective, status, completed,
   remaining, blockers, relevant files, one next recommended step.
2. `Durable knowledge` → hand off to `knowledge-vault` per that skill and its
   significance test: would losing this cause rediscovery, contradiction,
   drift, or repeated wasted work? If yes, update the canonical note and link
   it. If no, leave it in the session only.
3. `Temporary working context` → this is the only material the compacted
   summary may own outright.
4. `Discard` → repeated, obsolete, irrelevant, or duplicated detail. Drop it.

### 2. Write the compacted summary

Use this shape and nothing larger:

```text
# Compacted Context — <short topic>

Date:
Branch/Commit:

## Goal
## Constraints and User Decisions (Decided)
## Verified Facts (Verified, with file paths)
## Assumptions (Assumed — needs verification)
## Completed
## Unresolved / Open
## Active Files
## Next Action
## Handoff Pointers (WIP note, vault notes, session)
```

Keep it under one screen when possible. Link to the WIP note and vault notes
instead of copying their contents.

### 3. Hand off, do not duplicate

- WIP owns the next action and unfinished state. The summary points at it.
- The vault owns durable decisions and lessons. The summary points at them.
- The summary owns only the temporary bridge between them: what to hold in
  mind right now to continue.
- Never store the same fact authoritatively in all three systems. One owner,
  plus pointers.

### 4. Verify on use

A compacted summary is a hypothesis about prior state. Before acting on it,
spot-check the active files and the WIP next action against current source.
If the code moved on, discard the stale part and continue from the code.

## Pause and Resume Integration

`/pause` and `/resume` own the full procedure; this skill supplies the triage
step they need. Follow `work-in-progress` for the workflow itself, and use this
skill for the sorting that happens inside it.

For pause: compact the thread to identify what is unfinished, durable, or
disposable, then hand each category to its owner — unfinished work to WIP with
exactly one next action, durable knowledge to the vault while it is fresh, and
the temporary remainder to the compacted summary.

For resume: load `work-in-progress/current.md` first as the canonical resume
point, then only the vault notes that WIP points at, then inspect current
source. Source wins over the summary, WIP, and the vault alike; correct stale
notes rather than propagating them. Treat any prior compacted summary as a
low-authority hint and re-verify its assumptions before acting.

WIP plus the session note must be sufficient to resume even if the compacted
summary is lost. Do not restart settled investigation, and do not ask the user
to restate decisions already recorded in WIP or the vault.

## Anti-Duplication and Staleness Rules

- One fact, one owner: WIP for unfinished state, vault for durable knowledge,
  compaction for temporary recall. Everywhere else is a pointer.
- Search WIP and the vault (via `memory-index.md`) before writing; update the
  canonical note rather than creating a parallel copy.
- Mark conflicts `STALE`, `CONTRADICTED`, `AMBIGUOUS`, `UNVERIFIED`, or
  `HISTORICAL` per `knowledge-vault`, and resolve toward current source.
- Clear or archive WIP when work finishes so no stale resume point survives.
- Never keep two copies of a note (for example a "hot" and a "cold" version
  of the same topic). Temperature is retrieval order, not duplicated files.
- Session notes are history. The vault is durable state. The compacted summary
  is neither; it expires once work moves on.
