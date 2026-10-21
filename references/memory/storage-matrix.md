# Storage Matrix: Standard Cache Paths & Commands

<!-- last-verified: 2026-09-05 -->

Cross-platform map of standard developer cache locations, temp directories, and high-impact cleanup targets.

## Platform Environment Resolution

| Key | Windows (PowerShell) | macOS (POSIX) | Linux (POSIX) |
| :--- | :--- | :--- | :--- |
| **User Home** | `$env:USERPROFILE` | `$HOME` | `$HOME` |
| **Local AppData** | `$env:LOCALAPPDATA` | `~/Library/Caches` | `~/.cache` |
| **Roaming AppData** | `$env:APPDATA` | `~/Library/Application Support` | `~/.config` |
| **Temp Directory** | `$env:TEMP` | `/tmp` | `/tmp` |

---

## Developer Tooling Cache Paths

| Ecosystem | Path (Windows) | Path (macOS / Linux) | Native Clear Command |
| :--- | :--- | :--- | :--- |
| **VS Code Extensions** | `$env:USERPROFILE\.vscode\extensions` | `~/.vscode/extensions` | Manual semantic prune |
| **VS Code VSIX Cache** | `$env:APPDATA\Code\CachedExtensionVSIXs` | `~/Library/Application Support/Code/CachedExtensionVSIXs` | Direct folder deletion |
| **VS Code Workspaces** | `$env:APPDATA\Code\User\workspaceStorage` | `~/Library/Application Support/Code/User/workspaceStorage` | Targeted prune |
| **NPM Cache** | `$env:LOCALAPPDATA\npm-cache` & `$env:APPDATA\npm-cache` | `~/.npm` | `npm cache clean --force` |
| **NPX Cache** | `$env:LOCALAPPDATA\npm-cache\_npx` | `~/.npm/_npx` | Direct folder deletion |
| **PNPM Store** | `$env:LOCALAPPDATA\pnpm\store` | `~/.local/share/pnpm/store` | `pnpm store prune` |
| **Yarn Cache** | `$env:LOCALAPPDATA\Yarn\Cache` & `~/.yarn/berry/cache` | `~/.cache/yarn` & `~/.yarn/berry/cache` | `yarn cache clean` |
| **Bun Cache** | `$env:USERPROFILE\.bun\install\cache` | `~/.bun/install/cache` | `bun pm cache rm` |
| **Go Build Cache** | `$env:LOCALAPPDATA\go-build` | `~/.cache/go-build` | `go clean -cache` |
| **Electron Cache** | `$env:LOCALAPPDATA\electron\Cache` | `~/.cache/electron` | Direct folder deletion |
| **App Updaters** | `$env:LOCALAPPDATA\*updater*` | `~/Library/Caches/*updater*` | Safe pending installer purge |
| **Pip Cache** | `$env:LOCALAPPDATA\pip\cache` | `~/.cache/pip` | `pip cache purge` |
| **UV Cache (Python)** | `$env:LOCALAPPDATA\uv\cache` | `~/.cache/uv` | `uv cache clean` |
| **Composer Cache** | `$env:LOCALAPPDATA\Composer\cache` | `~/.cache/composer` | `composer clear-cache` |
| **Cargo Cache** | `$env:USERPROFILE\.cargo\registry\cache` | `~/.cargo/registry/cache` | Prune `.crate` archives |
| **Gradle Cache** | `$env:USERPROFILE\.gradle\caches` | `~/.gradle/caches` | Prune old wrapper versions |
| **Docker Build Cache** | Docker Engine internal | Docker Engine internal | `docker builder prune -f` |
| **HuggingFace Models** | `$env:USERPROFILE\.cache\huggingface\hub` | `~/.cache/huggingface/hub` | Manual candidate prune |
| **Torch Hub Models** | `$env:USERPROFILE\.cache\torch` | `~/.cache/torch` | Manual candidate prune |

---

## Operating System High-Yield Targets

### Windows
- **Hibernation File (`hiberfil.sys`)**:
  - Location: `C:\hiberfil.sys`
  - Size: Typically 40% to 100% of physical RAM (8 GB to 32 GB).
  - Toggled via Administrator prompt: `powercfg -h off` (instant reclamation).
- **NVIDIA App Driver Installer Cache**:
  - Location: `$env:ProgramData\NVIDIA Corporation\NVIDIA app`
  - Size: 2 GB to 10 GB of accumulated driver installers.
- **Delivery Optimization Cache**:
  - Location: `C:\Windows\SoftwareDistribution\DeliveryOptimization`
  - Cleaned via Windows Disk Cleanup / cleanmgr.

---

## Canonical Cache Signatures Pattern (The Universal Heuristic)

Modern desktop and developer applications (Electron, Chromium, VS Code, Slack, Discord, TikTok Studio, Obsidian, Spotify, etc.) share standard internal storage layouts. When performing signature-based decomposition, partition discovered directories strictly:

### Disposable Regenerable Signatures (Safe for Purge)

| Signature Pattern | Content Description | Safety Invariant |
| :--- | :--- | :--- |
| `Cache`, `Cache_Data`, `gecko_cache` | Web resource and network response caches | **100% Safe** (regenerates automatically) |
| `Code Cache` (`js`, `wasm`) | Pre-compiled V8 bytecode caches | **100% Safe** (re-compiled on demand) |
| `GPUCache`, `Dawn*Cache`, `*ShaderCache*` | Compiled GPU rendering shaders and textures | **100% Safe** (driver re-generates) |
| `Crashpad`, `crash_dump`, `*_dump` | Post-mortem diagnostic reports and mini-dumps | **100% Safe** (obsolete telemetry) |
| `logs`, `SessionLogs`, `watch_dog` | Historical runtime execution and debug logs | **100% Safe** (ephemeral diagnostics) |
| `*updater*\pending`, `ota-artifacts` | Downloaded staging installers and update packages | **100% Safe** (already applied or re-downloadable) |
| `CachedExtensionVSIXs`, `CachedData` | Temporary extension installer archives | **100% Safe** (installed extensions unaffected) |
| `app_shell_cache_*` | Cached desktop wrapper web views | **100% Safe** (ephemeral UI assets) |

### Inviolable User Data Signatures (STRICTLY FORBIDDEN FROM DELETING)

- **Authentication & Sessions**: `Login Data*`, `Cookies*`, `Sessions*`, `Session Storage*`, `Auth*`
- **Application Settings**: `User/settings.json`, `keybindings.json`, `Preferences`, `Local State`
- **Application Databases**: `Local Storage*`, `IndexedDB*`, `Web Data*`, `databases*`, `namespaces*`
- **User Assets & Projects**: `Bookmarks`, `extensions` (active version), `workspaces` (active state)

---

## Universal Stack Auto-Discovery Matrix

Dynamic detection patterns for developer environments across all attached drive volumes and operating systems without hardcoded path assumptions:

| Stack / Environment | Windows Discovery Rule | macOS / Linux Discovery Rule |
| :--- | :--- | :--- |
| **WampServer** | Look for `\wamp64\www` or `\wamp\www` on all `Get-PSDrive -PSProvider FileSystem` roots | N/A |
| **XAMPP** | Look for `\xampp\htdocs` on all file system drive roots | `/opt/lampp/htdocs` or `/Applications/XAMPP/htdocs` |
| **Laragon** | Look for `\laragon\www` on all file system drive roots | N/A |
| **MAMP** | Look for `\MAMP\htdocs` on all file system drive roots | `/Applications/MAMP/htdocs` |
| **Laravel Herd / Valet** | `$env:USERPROFILE\.config\herd` & `$env:USERPROFILE\Sites` | `~/Library/Application Support/Herd` & `~/Sites` |
| **System Web Server** | Look for `\inetpub\wwwroot` | `/var/www/html`, `/var/www`, `/srv/http` |
| **User Code Projects** | `$env:USERPROFILE\Projects`, `Development`, `workspace`, `repos`, `source` | `~/projects`, `~/development`, `~/workspace`, `~/Development`, `~/src` |
| **Custom Path** | Directly targeted via command argument (e.g. `/tidyfactor-cleaner <path>`) | Directly targeted via command argument |

---

## Universal Web & Framework Cache Signatures

| Ecosystem / Framework | Target Directory / Signature Pattern | Safety Rating | Recovery Command |
| :--- | :--- | :--- | :--- |
| **Next.js & Turbopack** | `.next\cache`, `.next\dev\cache\turbopack\*.sst`, `.next\cache\webpack\*.pack`, `.next\cache\images` | **100% Safe** | Automatic on `next dev` / `next build` |
| **Vite & Pre-bundler** | `node_modules\.vite`, `node_modules\.cache` | **100% Safe** | Automatic on `npm run dev` / `vite` |
| **Nuxt / Svelte / Astro** | `.nuxt`, `.output`, `.svelte-kit`, `.astro` | **100% Safe** | Automatic on dev/build |
| **E2E & Test Reports** | `playwright-report`, `test-results`, `cypress\videos`, `cypress\screenshots`, `coverage`, `.nyc_output` | **100% Safe** | Re-generated on test run |
| **Laravel Framework** | `storage\framework\cache\data`, `storage\framework\views`, `storage\framework\sessions` | **100% Safe** | `php artisan optimize:clear` |
| **Laravel Rotated Logs** | `storage\logs\laravel-*.log` | **100% Safe** | N/A (safe to truncate/purge) |
| **Symfony Dev Caches** | `var\cache\dev`, `var\cache\test`, `var\log` | **100% Safe** | `php bin/console cache:clear` |
| **WordPress Debug & Temp**| `wp-content\debug.log`, `wp-content\upgrade\*`, `wp-content\cache\*` | **100% Safe** | N/A |
| **WordPress Backup Dumps**| `wp-content\uploads\updraft\*`, `all-in-one-wp-migration-backups\*` | **Safe (with user confirmation)** | N/A |
| **Python & Bytecode** | `__pycache__`, `*.pyc`, `*.pyo`, `.pytest_cache`, `.ruff_cache`, `.mypy_cache` | **100% Safe** | Automatic on execution |
| **HuggingFace & Torch** | `~/.cache/huggingface/hub`, `~/.cache/torch` | **Safe (with user confirmation)** | Re-downloaded when called |
| **Prisma Query Engines** | `node_modules\.prisma\client\query-engine*` | **Safe** | `npx prisma generate` |
| **Docker Build Cache** | Docker daemon builder cache | **100% Safe** | `docker builder prune -f` |
| **Docker Dangling Images** | Untagged images and stopped build containers | **100% Safe** | `docker system prune` |
| **Superseded Project Zips** | `*.zip`, `*.tar.gz`, `*.bak` matching version tag/backup patterns | **Safe (with user confirmation)** | N/A |
| **Desktop Dist Outputs** | `dist\win-unpacked`, `dist\mac`, `dist\linux`, `build\dist` | **100% Safe (Dev)** | Rebuilt via project build script |
| **Lockfile-Backed Modules**| `node_modules` (when lockfile present), `.venv` (when requirements present) | **Safe to prune** | `npm i` / `pnpm i` / `pip install -r` |

### Strictly Protected Workspace Invariants (UNIVERSAL INVIOLABLE)

- **Local AI Models & Weights**: `*.gguf`, `*.bin`, `*.safetensors`, `*.onnx`, `models\`
- **Environment & Secret Files**: `.env`, `.env.*`, `wp-config.php`, `config.php`, `*.secret`, `credentials.json`
- **Version Control & Source Trees**: `.git`, active source directories (`src\`, `app\`, `components\`, `lib\`, `public\`)
- **Active Databases & Migrations**: `*.sqlite`, `*.db`, `migrations\`, `seeders\`


