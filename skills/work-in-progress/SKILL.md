---
name: work-in-progress
description: Preserve and resume unfinished development work. Use when work spans sessions, is blocked, is paused, the user says continue/resume/revisit, or meaningful implementation remains. Maintains WIP state, decisions, rejected approaches, blockers, testing status, files, and one clear next action.
---
# Work in Progress

## Create or Update WIP
Use `work-in-progress/WIP-short-topic.md` for meaningful unfinished work. Search for an existing related WIP first.

Include when applicable:
- Goal
- Current State
- Work Completed
- Remaining Work
- Current Approach
- Important Findings
- Discussion Context
- Decisions Made
- Rejected Approaches
- Files Being Worked On
- Problems / Blockers
- Testing / Verification
- Next Action
- Related Knowledge

Preserve conclusions and reasoning, not chat transcripts. Update on meaningful progress, not every small edit.

## Pause
When the user says pause, save where we are, continue later/tomorrow/next time, or equivalent:
1. Find/create the relevant WIP.
2. Record current state, completed and remaining work.
3. Preserve meaningful decisions, rejected approaches, blockers, and actual verification status.
4. Set one specific immediate `Next Action`.
5. Link relevant permanent knowledge.
6. Save worthwhile unresolved investigations under `questions/`.

## Resume
When the user says continue, resume, revisit, or finish previous work:
1. Find the matching WIP.
2. Read Goal, Current State, Completed, Remaining, Discussion Context, Decisions, Rejected Approaches, Blockers, Testing, related questions/knowledge, and Next Action.
3. Inspect current source code and verify the WIP still matches reality.
4. Continue from the documented stopping point instead of restarting investigation.
5. Update WIP as meaningful progress occurs.

## Complete
When WIP is finished:
1. Verify remaining work and testing.
2. Promote reusable findings to permanent knowledge.
3. Update architecture/ADRs/questions where needed.
4. Move unresolved follow-up into another WIP.
5. Remove the WIP from active state; archive as `Status: Completed` with date/outcome if historically useful, otherwise delete after knowledge extraction.
