# Workflow: clean-ide

One outcome: clean IDE extension stores (VS Code, Antigravity, Cursor) with obsolete versions and installer VSIXs removed, and process lock conflicts safely resolved.

---

## Steps

### Step 0: Auto-Sense IDE Extension Stores
1. Locate active extension directories:
   - VS Code: `~/.vscode/extensions`
   - Cursor: `~/.cursor/extensions`
   - Antigravity: `~/.gemini/antigravity-ide/extensions` or shared VS Code extensions.
2. Locate cached VSIX installer packages:
   - Windows: `$env:APPDATA\Code\CachedExtensionVSIXs`
   - macOS: `~/Library/Application Support/Code/CachedExtensionVSIXs`
   - Linux: `~/.config/Code/CachedExtensionVSIXs`

### Step 1: Semantic Version Discovery & Duplicate Grouping
1. Scan all folders matching pattern `{publisher}.{extensionName}-{version}`.
2. Group folders by `{publisher}.{extensionName}`.
3. For any group containing $\ge 2$ directories:
   - Mark the highest semantic version (or newest timestamp) as **Active** (`KEEP`).
   - Mark all older versions as **Obsolete** (`CANDIDATE_FOR_PURGE`).
4. Scan for abandoned dot-folders (e.g. `.ed9f2287...`) left behind by interrupted installations.
5. Cross-reference with the `.obsolete` registry file.

### Step 2: Process Lock Inspection & Termination
Before attempting file deletion:
1. Check if any running process has open handles inside the target directory:
   - Windows: Check process command lines for paths matching the candidate folder:
     `Get-CimInstance Win32_Process | Where-Object { $_.CommandLine -like "*$candidateFolder*" }`
2. If locking processes exist (e.g. orphaned `codex-code-mode-host.exe`, `kilo.exe`, language servers):
   - Report the PID, process name, and parent process to the user.
   - Terminate the orphaned daemon process (`Stop-Process -Id <PID> -Force`).
   - Allow 1-2 seconds for file system handles to release.

### Step 3: Deletion & Registry Maintenance
1. Remove confirmed obsolete extension directories.
2. Purge `CachedExtensionVSIXs` directory.
3. Clean the `.obsolete` JSON file to remove keys corresponding to deleted folders, or delete if empty.

---

## Validation Checklist

- [ ] Active/highest extension version remains 100% intact and untouched.
- [ ] No `Access Denied` errors occur during deletion (lock verification passed).
- [ ] `.obsolete` registry synchronized with actual disk state.
- [ ] Exact MB freed verified and reported to the user.
