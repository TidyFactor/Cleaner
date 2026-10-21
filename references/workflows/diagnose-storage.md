# Workflow: diagnose-storage

One outcome: a fast, non-blocking storage report categorizing reclaimable gigabytes across IDEs, browsers, developer tools, and OS temporary stores without hanging system I/O.

---

## Steps

### Step 0: Context Auto-Sensing (Rule 14)
1. Detect host operating system:
   - Windows: PowerShell (`$IsWindows` or `$env:OS`).
   - macOS / Linux: Bash (`uname -s`).
2. Identify root drives or mounts and free space ratio:
   - Windows: `Get-PSDrive -PSProvider FileSystem | Select-Object Name, Free, Used`.
   - Linux / macOS: `df -h /`.
3. If free space is below 5%, flag as `CRITICAL EMERGENCY`.
4. Dynamically detect developer project roots and web server stacks across all mounted volumes using the **Universal Stack Auto-Discovery Matrix** (in `storage-matrix.md`), or accept user-provided target directory directly.

### Step 1: Dynamic Heavy-Hitters Profiling (Fast Tier-0 Triage)
Instead of relying exclusively on static path lists, execute a fast non-blocking triage of top-level consumers in user application stores:
- **Windows**: Scan immediate children of `$env:LOCALAPPDATA` and `$env:APPDATA` for directories exceeding size threshold ($\ge 100$MB) using Robocopy fast summary mode with `/R:0 /W:0`:
  `robocopy <subfolder> "$env:TEMP\nul_dest" /L /E /NJH /BYTES /NFL /NDL /NC /NS /XJ /R:0 /W:0 2>$null`
- **Linux / macOS**: Query top consumers:
  `du -sh ~/.cache/* ~/.config/* 2>/dev/null | sort -hr | head -n 25`

### Step 1.1: Web Workspace & Multi-Project Fast Exclusion Triage
When analyzing a developer project root (e.g. web server root, monorepo, or project directories):
- **Never perform unbounded recursive directory traversals into dependency stores**: Traversing millions of deep files inside `node_modules` or `vendor` will freeze CLI execution.
- **Windows (PowerShell + Robocopy Exclusion Mode)**:
  ```powershell
  robocopy <targetRoot> "$env:TEMP\nul_dest" /L /S /BYTES /XJ /XD node_modules vendor .git /MIN:15728640 /FP /NDL /NJH /NJS /NC /R:0 /W:0 2>$null
  ```
- **Linux / macOS (POSIX Shell + find Pruning)**:
  ```bash
  find "<targetRoot>" -type d \( -name node_modules -o -name vendor -o -name .git \) -prune -o -type f -size +15M -exec ls -lh {} + 2>/dev/null
  ```
- Classify discovered assets strictly into the **Universal 4-Tier Project Matrix**:
  - **Tier 1 (100% Safe)**: Framework & build caches (`.next/cache`, Turbopack `.sst`, Webpack `.pack`, `.vite`, `.nuxt`, `storage/framework/cache/data`, Blade views, `__pycache__`).
  - **Tier 2 (Superseded Archives)**: Version-tagged or duplicate zip/tar.gz files and abandoned backups.
  - **Tier 3 (App Dist Outputs)**: Electron/Tauri `win-unpacked`, `dist/mac`, and duplicate compiled binaries.
  - **Tier 4 (Lockfile Dependencies)**: `node_modules` or `.venv` with active lockfiles.
  - **INVIOLABLE**: Local AI weights (`.gguf`, `.safetensors`), `.env` secrets, source code, and live databases.

### Step 2: Signature-Based Decomposition & Measurement
Deconstruct discovered heavy directories against the **Canonical Cache Signatures Pattern** (defined in `storage-matrix.md`):
1. **Core Developer Tooling**: `.vscode/extensions`, `.gemini/antigravity-ide`, AppData / `.cache` IDE caches.
2. **Package & Build Stores**: `npm-cache` (Local & Roaming), `go-build`, `electron\Cache`, pip, composer, cargo.
3. **Application Updaters & Caches**: `*updater*\pending`, `app_shell_cache_*`, Electron and Chromium application caches (`Cache`, `Code Cache`, `GPUCache`, `Dawn*Cache`, `ShaderCache`).
4. **Crash Dumps & Telemetry Logs**: `Crashpad`, `crash_dump`, `*_dump`, `logs`, `SessionLogs`.
5. **OS / Driver Tiers**: Temp directories (`$env:TEMP`), GPU installer packages (`ota-artifacts`), Windows hibernation (`hiberfil.sys`).

### Step 3: Context Delta & Structured Presentation
Calculate:
$$\text{Reclaimable Space} = \sum \text{Identified Safe Caches}$$
Present findings in a structured table and provide clear interactive choices:
- `[A] Recommended Safe Cleanup`: Purge obsolete extension versions, browser caches, and agent recordings.
- `[B] Dev Tools & Package Cache Cleanup`: Purge package manager caches and temp builds.
- `[C] Deep System Clean`: OS hibernation toggle and driver installer packages.
- `[D] Custom Selection`: User picks specific components.

---

## Validation Checklist

- [ ] Drive free space checked without running unbounded recursive searches.
- [ ] Tiers categorized strictly into (Safe Regenerable / Dev Caches / System).
- [ ] User presented with exact MB/GB projections before any destructive operation.
- [ ] Zero hardcoded username paths used in commands.
