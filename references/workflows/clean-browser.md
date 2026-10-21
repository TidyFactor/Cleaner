# Workflow: clean-browser

One outcome: clean Chromium/Chrome/Edge browser cache stores across all user profiles with 100% preservation of authentication, logins, sessions, and bookmarks.

---

## Steps

### Step 0: Auto-Sense Browser Profiles & Running State
1. Locate User Data root directory dynamically:
   - Google Chrome:
     - Windows: `$env:LOCALAPPDATA\Google\Chrome\User Data`
     - macOS: `~/Library/Application Support/Google/Chrome`
     - Linux: `~/.config/google-chrome`
   - Microsoft Edge:
     - Windows: `$env:LOCALAPPDATA\Microsoft\Edge\User Data`
2. Parse `Local State` JSON to identify all profiles (`Default`, `Profile 1`, `Profile 2`, ...).
3. Check if browser processes are running:
   - Windows: `Get-Process chrome, msedge -ErrorAction SilentlyContinue`
   - macOS / Linux: `pgrep -i chrome || pgrep -i msedge`

### Step 1: Process Coordination
If the browser is running:
- Never purge SQLite-backed caches while the browser is actively writing (avoids write locks and database corruption).
- Interactively request user confirmation to close the browser cleanly, or stop background renderers after user consent:
  `Stop-Process -Name chrome -Force`

### Step 2: Targeted Cache Isolation (Strict Invariant Boundary)
Traverse each profile folder and target **ONLY** disposable regenerable subdirectories:
- `Cache` and `Cache_Data`
- `Code Cache\js` and `Code Cache\wasm`
- `GPUCache`
- `DawnCache`, `DawnGraphiteCache`, `DawnWebGPUCache`
- `ShaderCache` and `GrShaderCache`
- `Service Worker\CacheStorage` and `Service Worker\ScriptCache`
- `Crashpad\reports`
- `component_crx_cache`

**STRICTLY FORBIDDEN FROM DELETING**:
- `Login Data` (passwords)
- `Cookies` (session tokens)
- `Bookmarks`
- `Preferences`
- `History`
- `Web Data`
- `Extensions` directory

### Step 3: Purge Execution & Integrity Verification
1. Remove targeted cache directories using silent error suppression for transient file locks.
2. **Deterministic Verification Loop**: Check every discovered profile to confirm that `Login Data`, `Preferences`, and `Bookmarks` remain `INTACT`.
3. Calculate and display total megabytes freed and new User Data size.

---

## Validation Checklist

- [ ] Browser process was gracefully handled before file deletion.
- [ ] Only regenerable cache directories were deleted.
- [ ] Post-clean integrity check passes on all profile database files.
- [ ] Zero user logins or saved credentials lost.
