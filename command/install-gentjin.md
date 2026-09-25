---
description: Install GENTJIN into the global OpenCode configuration by following INSTALL.md, with backups, guarded vault migration, and verification.
---

Install GENTJIN by following `INSTALL.md` in the current repository.

Treat this as a first-time installation. Resolve the source repository, `<opencode-root>` (`<user-home>/.config/opencode`), and `<gentjin-home>` (`<opencode-root>/GentJin`) exactly as the runbook specifies, then execute every step in order: preflight, timestamped backup, permanent home, runtime view, guarded legacy Knowledge Vault migration, additive global permission, verification, and the manifest.

Do not improvise a shorter path. Never create `install.ps1`, `install.sh`, or another installer script, and never copy the source `opencode.jsonc`, dependency folders, package metadata, credentials, or generated caches. Never delete or weaken user-owned configuration; batch one concise approval request for any conflict.

If a previous GENTJIN installation already exists, use `/update-gentjin` instead of restarting the architecture.

Print the runbook's final summary, including the permanent home, OpenCode root, installed skill and command counts, backup path, vault state, permission state, and skipped conflicts, then remind the user to restart OpenCode.
