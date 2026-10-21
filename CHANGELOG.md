# Changelog

All notable changes to the `tidyfactor-cleaner` skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0] - 2026-10-21

### Added
- **Universal Multi-Project & Web Stack Auto-Discovery**: Dynamic detection of web server environments across Windows, macOS, and Linux without hardcoded assumptions (supporting WAMP, XAMPP, Laragon, MAMP, Laravel Herd, Valet, LAMP, and user workspace folders).
- **Universal Framework Cache Signatures**: Full coverage of framework-specific bloat across modern ecosystems:
  - *Frontend*: Next.js (`.next/cache`, Turbopack `.sst`, image optimization), React/Vite (`node_modules/.vite`), Nuxt, SvelteKit, Astro, and E2E test/coverage stores (`playwright-report`, `cypress/videos`, `coverage`).
  - *PHP*: Laravel surgical hygiene (`storage/framework/cache/data`, Blade views, sessions, giant rotated `laravel-*.log` files), Symfony (`var/cache`), and WordPress local debug logs (`wp-content/debug.log`) and backup plugin dumps.
  - *Package Managers*: Comprehensive prune support for PNPM (`pnpm store prune`), Yarn (`yarn cache clean`), Bun (`bun pm cache rm`), and NPX cache (`_npx`).
  - *Python & AI*: Bytecode pruning (`__pycache__`), test caches (`.pytest_cache`, `.ruff_cache`), and model store hygiene (`~/.cache/huggingface/hub`, `torch`).
  - *Docker*: Build cache reclamation (`docker builder prune`) and dangling resources (`docker system prune`).
- **CLI-First Orchestration with Surgical Fallback**: Invokes official framework cleaning tools natively (e.g. `php artisan optimize:clear`, `composer clear-cache`, `npm cache clean --force`, `go clean -cache`) with automatic fallback to filesystem signature pruning if the runtime CLI is absent.
- **Cross-Platform Fast Exclusion Triage**: High-speed discovery using Robocopy exclusion mode (`/XD node_modules vendor .git /MIN:<bytes>`) on Windows and POSIX `find ... -prune -o -size +15M` on macOS/Linux to analyze massive code repositories in milliseconds without I/O freezes.
- **Language Server & MCP Daemon Lock Management**: Added signatures for `gopls.exe`, `tsserver`, `intelephense`, and `rust-analyzer` with resolution discipline for `unlinkat / Access is denied` errors.
- **Universal 4-Tier Project Safety Matrix**: Strict separation between disposable build artifacts, superseded archives, distributables, and lockfile-backed dependencies, with absolute protection for local AI weights (`.gguf`, `.safetensors`), `.env` secrets, source code, and databases.

## [1.2.0] - 2026-10-19

### Added
- **Dynamic Heavy-Hitters Profiler** (Fast Tier-0 Triage) in `diagnose-storage.md` to automatically scan top-level consumers in `$env:LOCALAPPDATA` / `$env:APPDATA` ($\ge 100$MB) without hardcoded blind spots.
- **Canonical Cache Signatures Pattern** in `storage-matrix.md` establishing universal heuristic recognition for regenerable caches (`Cache`, `Code Cache`, `GPUCache`, `Dawn*Cache`, `Crashpad`, `*updater*`, `logs`) versus inviolable user data (`User`, `settings.json`, `Preferences`, `Cookies`, `Login Data`, `Bookmarks`).
- **Safe Deletion Execution Pattern** in `lock-matrix.md` documenting PowerShell child item iteration, `-LiteralPath` wildcard avoidance, graceful locked handle skipping, and pre/post drive delta verification.
- Expanded `clean-devtools.md` to cover Go build cache, Electron prebuilt binary downloads, desktop application updaters, and dual-location npm caches.

## [1.1.0] - 2026-10-19

### Added
- Added `$env:LOCALAPPDATA\npm-cache` alongside Roaming path in `storage-matrix.md` to capture modern Windows npm cache locations.
- Added Go build cache path (`$env:LOCALAPPDATA\go-build` / `~/.cache/go-build`) to developer tooling matrix.
- Added Electron binary cache (`$env:LOCALAPPDATA\electron\Cache` / `~/.cache/electron`) to developer tooling matrix.
- Added Desktop Application Updater installer stores (`$env:LOCALAPPDATA\*updater*` / `~/Library/Caches/*updater*`) for safe pending installer cleanup.

### Fixed
- Fixed Robocopy fast list-mode diagnostic command in `diagnose-storage.md` by explicitly appending `/R:0 /W:0` flags to prevent hanging indefinitely on locked process handles in temp/cache directories.

## [1.0.0] - 2026-09-05

### Added
- Initial release of `tidyfactor-cleaner` skill conforming to TidyFactor 14 Structural Rules.
