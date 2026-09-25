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

Never copy, merge, or modify `opencode.jsonc`. It is user configuration, not a
GENTJIN payload. Preserve all other providers, plugins, settings, permissions,
MCP configuration, authentication data, and unrelated files.

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
5. Apply the manifest table above.
6. Record unrelated existing files so they can be checked after installation.

Do not read or print credentials or unrelated configuration contents.

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
```

Back up an existing `install-manifest.json` before replacing it. Do not back up
identical files or unrelated files. If a backup fails, stop before changing
either destination.

### 3. Populate the permanent home

Create `<gentjin-home>` if needed. Copy the approved source payload into it,
preserving relative paths. Copy rather than move so the clone remains unchanged.

Install missing source files automatically. Update unchanged manifest-managed
permanent files automatically after backup. Ask only for the unsafe cases in the
manifest table. Preserve unknown files already present in the permanent home.
If a permanent source file is skipped, do not synchronize its runtime
counterpart.

Do not copy `opencode.jsonc`, dependency directories, package metadata,
credentials, or generated caches.

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
unrelated files, plugins, settings, or configuration. Do not modify
`<opencode-root>/opencode.jsonc`.

### 5. Verify both layers

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
- `opencode.jsonc` was not copied or modified;
- no credentials or environment values were read, printed, or written;
- no temporary staging files remain.

If verification fails, do not report success.

### 6. Write the manifest last

After both layers verify successfully, create or update
`<gentjin-home>/install-manifest.json` with the final hashes and managed paths.
The manifest is finalized only after the permanent and runtime files have been
verified. If manifest creation fails, report the installation as incomplete.

Print a concise summary containing the resolved permanent home, OpenCode root,
installed skill and command counts, backup path (or `None`), and any skipped
unsafe conflicts. End with:

```text
Restart OpenCode to activate GENTJIN.
```

## Preserve the Knowledge Vault

Do not create, move, migrate, or delete a Knowledge Vault during installation.
The `knowledge-vault` skill must continue to resolve project knowledge from the
current user's home directory:

```text
<user-home>/Documents/KnowledgeVault/<project-identifier>/
```

Create or populate a project vault only when that project needs it and normal
permissions allow it.

## Future maintenance

The permanent home and `install-manifest.json` are sufficient for later update,
repair, and uninstall workflows. Do not implement those workflows during this
installation.

When testing this runbook, use a temporary source and temporary OpenCode
configuration directory. Never test against the active user's global OpenCode
configuration.
