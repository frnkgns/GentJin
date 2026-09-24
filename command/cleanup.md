---
description: Full pre-main-branch cleanup, QA, debugging, testing, optimization, and production-readiness review, ending with a cleanup report.
---

Perform a full `clean up` per the `# Clean Up Command` section of the global instructions: cleanup, QA, debugging, testing, testing/optimization, and production-readiness review of the current source-control changes. Also run the knowledge-side checks (inbox triage, WIP and question consistency).

`clean up` means the work is being finished, so it always ends with a report: save the detailed cleanup report into the vault under `reports/` and print a concise console report per the `# Response and Report Format` section of the global instructions.

If there are no pending source-control changes, say so and still run the knowledge-side checks. If the directory is not a Git repository, state that git-based steps were skipped and continue with the checks that still apply.