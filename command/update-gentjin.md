---
description: Update an existing GENTJIN installation from the current repository using the manifest, with backups, prompts for unsafe files, and verification.
---

Update GENTJIN by following `INSTALL.md` in the current repository.

Use `<gentjin-home>/install-manifest.json` as the ownership record. Update manifest-managed files whose recorded hash matches the installed hash, back them up first, and ask before replacing any file that is locally modified, unmanaged, or ambiguous. Preserve unknown files in both the permanent home and the runtime view.

Re-run the additive global `opencode.jsonc` permission check and report whether it was `added` or `already present`. Skip the vault migration when `<user-home>/Documents/KnowledgeVault` already exists, and never merge vault directories.

Verify both layers, then write the manifest last. Report the installed skill and command counts, backup path, vault state, permission state, and any skipped unsafe conflicts, then remind the user to restart OpenCode.
