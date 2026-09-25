# GENTJIN Agent-Assisted Installation

Use this runbook when the user says:

```text
Install GENTJIN by following INSTALL.md
```

Perform the installation with OpenCode's available file tools and platform APIs.
Do not create or execute `install.ps1`, `install.sh`, or another traditional
installer script.

## Architecture

Keep these locations separate:

```text
<source-root>       The cloned GENTJIN repository
<opencode-root>     <user-home>/.config/opencode
<gentjin-home>      <opencode-root>/GentJin
```

The permanent GENTJIN home contains the distributable source and reference
files. OpenCode discovers the runtime view in the parent configuration root:

```text
<opencode-root>/AGENTS.md
<opencode-root>/command/
<opencode-root>/skills/
```

The permanent home is the source of truth for future maintenance. The runtime
files are synchronized copies.

## Resolve paths

Detect the current operating system at runtime. Resolve the current user's home
directory without using a username or a path from the cloned repository:

- Windows: use `$env:USERPROFILE`, or the platform user-profile API when it is
  unavailable.
- macOS/Linux: use `$HOME`, or the operating system's user-home API when it is
  unavailable.
- If the home directory cannot be resolved confidently, stop and ask the user
  without changing files.

Resolve:

```text
<opencode-root> = <user-home>/.config/opencode
<gentjin-home> = <opencode-root>/GentJin
```

Use the platform's native path separator. If `OPENCODE_CONFIG` or another
setting points to a nonstandard configuration, report it and ask before using
that location. Never install into a project-local configuration by accident.

If either destination exists as a file instead of a directory, stop and report
the conflict.

## Resolve the source

Start at the current working directory. Walk upward, if needed, to the nearest
directory containing `AGENTS.md`, `command/`, and `skills/`. Stop if the source
is ambiguous or incomplete.

If the source is already `<gentjin-home>`, use it as the source of truth. If the
source is `<opencode-root>` but not `<gentjin-home>`, stop and ask before
performing maintenance.

The source payload is:

```text
AGENTS.md
README.md
INSTALL.md
command/
skills/
LICENSE                 When present
```

Preserve relative paths and additional files inside `command/` or `skills/`.
Do not copy `.git/`, `node_modules/`, package metadata, temporary files,
credentials, `.env` files, or generated caches into the permanent home.

Never copy the source `opencode.jsonc` into either destination. It is user
configuration, not a GENTJIN payload. Preserve all providers, plugins,
settings, permissions, MCP configuration, authentication data, and unrelated
files in the user's global `opencode.jsonc`, except for the additive permission
change defined below.

## Add the required global permission

GENTJIN needs one explicit OpenCode permission so it can read and write the
Knowledge Vault outside the active project:

```jsonc
{
  "permission": {
    "external_directory": {
      "~/Documents/KnowledgeVault/**": "allow"
    }
  }
}
```

Apply this rule only to `<opencode-root>/opencode.jsonc`:

- If `~/Documents/KnowledgeVault/**` already maps to `allow`, keep the file
  unchanged.
- If the path is missing and `permission.external_directory` already exists as
  an object, append the rule without changing any existing entry, pattern, or
  order.
- If `permission` or `permission.external_directory` is missing, add only the
  required structure around the existing configuration.
- If the path exists with a different action, or the addition would require
  replacing, removing, reordering, or weakening any existing entry, stop and
  ask the user for explicit approval in one concise request.
- Never delete, disable, replace, reorder, or downgrade existing configuration
  without explicit user approval. Never copy the source `opencode.jsonc` over
  the global file.
- If the global file is missing, stop and ask before creating it.
- Read only the structure needed for this merge. Never print, copy, log, or
  record secrets, tokens, credentials, or unrelated configuration values.
- Do not add `opencode.jsonc` to `install-manifest.json`; it remains
  user-owned.

The addition must be idempotent. A repeated install or update must not create
duplicate rules, reformat unrelated content, or rewrite the file when the
permission already exists.

## Use the manifest for ownership

`<gentjin-home>/install-manifest.json` is the GENTJIN ownership record. Create
or update it after each successful installation. Do not list the manifest itself
as a managed file.

Record, for every installed file:

- layer: `permanent` or `runtime`;
- path relative to that layer;
- SHA-256 hash captured after the successful install or update;
- manifest format version and update timestamp.

Use only relative managed paths in the manifest. Do not record credentials or
repository-owner paths.

Classify every destination file with this table:

| Destination state | Action |
| --- | --- |
| Missing expected GENTJIN file | Install automatically; no prompt |
| Manifest-managed and hash matches the recorded hash | Back up and update automatically; no prompt |
| Manifest-managed and hash differs | Treat as locally modified; ask before replacing |
| Existing but not manifest-managed and byte-identical to source | Skip and leave unmanaged; no prompt |
| Existing but not manifest-managed and different | Treat as user-owned or ambiguous; ask before replacing |
| Invalid manifest, unreadable file, or file/directory type mismatch | Stop that path and ask |

Apply the same rules to `AGENTS.md`; it is not automatically user-owned. Batch
all required confirmations into one concise request. Never prompt for missing
files or unchanged manifest-managed files during a normal install or update.

## Install safely

### 1. Preflight

Before writing anything:

1. Resolve the source, `<opencode-root>`, and `<gentjin-home>`.
2. Read the existing manifest, if present.
3. Enumerate the source payload directly; do not depend on a permanent-home copy
   that may not exist yet.
4. Compare source files directly with both permanent-home and runtime
   destinations.
5. Inspect only the minimum structure of `<opencode-root>/opencode.jsonc` needed
   to determine whether the required permission is present, conflicting, or
   missing.
6. Detect whether `<user-home>/Documents/ObsidianVault/` exists and
   `<user-home>/Documents/KnowledgeVault/` is absent. Do not open, merge, or
   delete vault contents during detection.
7. Apply the manifest table above.
8. Record unrelated existing files so they can be checked after installation.

Do not read or print credentials. Never print, copy, or log unrelated
configuration values; verify structure locally and report only the required
permission state.

### 2. Back up planned changes

Before the first write, create a timestamped directory under the OpenCode root:

```text
<opencode-root>/gentjin-backup/<yyyy-MM-dd-HHMMSS>/
```

Generate the timestamp from the current clock. Back up every existing file that
will be replaced or automatically updated, preserving its layer and relative
path:

```text
<backup>/permanent/AGENTS.md
<backup>/runtime/AGENTS.md
<backup>/runtime/command/cleanup.md
<backup>/global/opencode.jsonc
<backup>/obsidian/obsidian.json
```

Back up `<opencode-root>/opencode.jsonc` before an approved permission change.
Back up Obsidian's saved vault registry before an approved vault-path change.
Back up an existing `install-manifest.json` before replacing it. Do not back up
identical files or unrelated files. If a backup fails, stop before changing
either destination, the global configuration, or the vault path.

### 3. Populate the permanent home

Create `<gentjin-home>` if needed. Copy the approved source payload into it,
preserving relative paths. Copy rather than move so the clone remains unchanged.

Install missing source files automatically. Update unchanged manifest-managed
permanent files automatically after backup. Ask only for the unsafe cases in the
manifest table. Preserve unknown files already present in the permanent home.
If a permanent source file is skipped, do not synchronize its runtime
counterpart.

Do not copy the source `opencode.jsonc`, dependency directories, package
metadata, credentials, or generated caches. The additive permission change to
the global configuration is a separate step below.

### 4. Synchronize the runtime view

After the permanent files are present, copy only the runtime payload into the
OpenCode root:

```text
<gentjin-home>/AGENTS.md       -> <opencode-root>/AGENTS.md
<gentjin-home>/command/**      -> <opencode-root>/command/**
<gentjin-home>/skills/**       -> <opencode-root>/skills/**
```

Install missing runtime files automatically. Back up and update unchanged
manifest-managed runtime files automatically. Ask before replacing locally
modified, unmanaged, ambiguous, or otherwise unsafe files.

Preserve extra files in existing skill and command directories. Never remove
unrelated files, plugins, settings, or configuration. The global
`opencode.jsonc` is not part of the runtime payload; change it only in the next
step.

### 5. Migrate the legacy Knowledge Vault path

When `<user-home>/Documents/ObsidianVault/` exists and
`<user-home>/Documents/KnowledgeVault/` does not, batch one explicit approval
request with the other install questions. Continue only after the user approves.

1. Verify the legacy path is a directory and the canonical path does not exist.
   If both paths exist, stop and ask. Never merge the two directories.
2. Verify Obsidian is closed. If it is running, stop and ask the user to close
   it before continuing so the application cannot rewrite its saved paths.
3. Record the vault entry count and confirm `.obsidian/` exists.
4. Rename only `<user-home>/Documents/ObsidianVault/` to
   `<user-home>/Documents/KnowledgeVault/`. Do not copy, merge, or delete vault
   content, and do not move individual notes.
5. With separate explicit approval, update only Obsidian's saved vault path.
   Never modify the Obsidian application installation or unrelated application
   folders.
6. Verify the canonical directory, the entry count, `.obsidian/`, and the saved
   Obsidian path after the rename.

If the user declines, leave the legacy path unchanged, record the migration as
`skipped`, and continue. Never retry a skipped migration automatically in the
same installation.

### 6. Add the required global permission

After both layers are populated and backed up, update
`<opencode-root>/opencode.jsonc` only when the preflight check shows the
required permission is missing and the addition is safe under the rules above.

Use a minimal, targeted edit. Do not rewrite, reformat, or reserialize the whole
file. Do not add the file to the manifest. If the preflight check found a
conflict, stop and ask instead of editing.

### 7. Verify the installation

Verify every source payload file in the permanent home and every runtime file in
the OpenCode root. At minimum, confirm:

```text
AGENTS.md
command/cleanup.md
command/report.md
command/status.md
skills/cleanup/SKILL.md
skills/database-review/SKILL.md
skills/frontend-review/SKILL.md
skills/git-workflow/SKILL.md
skills/knowledge-vault/SKILL.md
skills/reporting/SKILL.md
skills/security-review/SKILL.md
skills/work-in-progress/SKILL.md
```

Also confirm:

- installed files match the source byte-for-byte;
- unrelated files recorded during preflight still exist;
- no unrelated skill, command, plugin, or configuration was removed;
- the source `opencode.jsonc` was not copied into either layer;
- the global `opencode.jsonc` contains the required permission, or was already
  correct and left unchanged;
- every pre-existing global configuration entry is still present, and nothing
  was removed, replaced, reordered, or downgraded;
- `opencode.jsonc` is not listed in `install-manifest.json`;
- the legacy vault was renamed only when approved, and its entry count and
  `.obsidian/` metadata were preserved;
- Obsidian's saved path was updated only when approved, and the Obsidian
  application installation was not modified;
- no credentials or environment values were read, printed, or written;
- no temporary staging files remain.

If verification fails, do not report success.

### 8. Write the manifest last

After both layers verify successfully, create or update
`<gentjin-home>/install-manifest.json` with the final hashes and managed paths.
The manifest is finalized only after the permanent and runtime files have been
verified. If manifest creation fails, report the installation as incomplete.

Print a concise summary containing the resolved permanent home, OpenCode root,
installed skill and command counts, backup path (or `None`), the vault migration
state (`renamed`, `already canonical`, or `skipped` after user approval), the
global configuration permission state (`added`, `already present`, or `skipped`
after user approval), and any skipped unsafe conflicts. End with:

```text
Restart OpenCode to activate GENTJIN.
```

## Keep the Knowledge Vault canonical

The `knowledge-vault` skill must resolve project knowledge from the current
user's home directory:

```text
<user-home>/Documents/KnowledgeVault/<project-identifier>/
```

Installation may rename the legacy `<user-home>/Documents/ObsidianVault/`
directory to that canonical location only when the legacy directory exists,
the canonical directory is absent, the user explicitly approves the rename, and
Obsidian is closed. Never merge directories, delete vault content, or modify the
Obsidian application installation. Update only Obsidian's saved vault path, and
only when explicitly approved.

Create or populate a project vault only when that project needs it and normal
permissions allow it. Granting the required OpenCode permission does not create,
move, migrate, or delete any vault.

## Future maintenance

The permanent home and `install-manifest.json` are sufficient for later update,
repair, and uninstall workflows of GENTJIN-managed files. Re-run the additive
global permission check on every update; the global `opencode.jsonc` remains
user-owned and is not represented in the manifest. The vault migration is a
one-time rename: after a successful migration, do not attempt it again, and if
both the legacy and canonical paths exist, stop and ask. Do not implement
maintenance workflows during this installation.

When testing this runbook, use a temporary source and temporary OpenCode
configuration directory. Never test against the active user's global OpenCode
configuration.
