# Workflow: clean-devtools

One outcome: clean development tool caches, package manager staging directories, and orphaned build artifacts across ecosystems.

---

## Steps

### Step 0: Auto-Sense Installed Package Managers & Framework Tools
Probe for installed CLIs dynamically across PATH:
- `npm` / `pnpm` / `yarn` / `bun`
- `php` / `composer`
- `python` / `uv` / `poetry`
- `go` / `cargo`
- `docker`

### Step 1: Framework & Package Manager Orchestration (CLI-First with Surgical Fallback)
Always prefer native deterministic cleaning commands over raw file deletion when the toolchain is active:

1. **JavaScript & Frontend Frameworks**:
   - **NPM / NPX**: Run `npm cache clean --force`, purge `$env:LOCALAPPDATA\npm-cache\_npx` or `~/.npm/_npx`.
   - **PNPM**: Run `pnpm store prune` to release unreferenced packages from hardlink store.
   - **Yarn (Berry / Classic)**: Run `yarn cache clean`.
   - **Bun**: Run `bun pm cache rm` or purge `~/.bun/install/cache`.
   - **Next.js Projects**: Purge `.next/cache` (images, fetch, turbopack `.sst`, webpack packs) safely without touching `.env*` or source files.
   - **Vite Projects**: Purge `node_modules/.vite` stale pre-bundled dependency artifacts.
   - **Test & E2E Artifacts**: Purge `playwright-report/`, `test-results/`, `cypress/videos/`, `coverage/`.

2. **PHP, Laravel & Web Stacks**:
   - **Laravel**:
     - *CLI-First*: If `php artisan` exists in project root, execute `php artisan optimize:clear` (clears cache, views, routes, config).
     - *Surgical Fallback*: Purge contents of `storage/framework/cache/data`, `storage/framework/views`, and truncate overgrown `storage/logs/laravel-*.log` files to 0 bytes.
   - **Symfony**: Run `php bin/console cache:clear` or purge `var/cache/dev`.
   - **Composer**: Run `composer clear-cache` or purge Composer cache directory.
   - **WordPress**: Truncate/purge `wp-content/debug.log`, clear `wp-content/upgrade/` and `wp-content/cache/`.

3. **Go Compiler & Language Server**:
   - *Lock Handling*: Check for active `gopls` processes locking cache binaries.
   - Run `go clean -cache`. If locked, offer to release `gopls` or safely purge unlocked objects.

4. **Python & AI Ecosystem**:
   - *Package Managers*: Run `uv cache clean`, `pip cache purge`, or `poetry cache clear`.
   - *Bytecode & Tool Caches*: Purge `__pycache__`, `.pytest_cache`, `.ruff_cache`, `.mypy_cache`.
   - *AI Models*: Triage `~/.cache/huggingface/hub` and `~/.cache/torch` with user confirmation before pruning obsolete models.

5. **Rust & Electron**:
   - *Cargo*: Prune `~/.cargo/registry/cache` archives.
   - *Electron*: Purge cached prebuilt runtime binaries in `$env:LOCALAPPDATA\electron\Cache` or `~/.cache/electron`.

6. **Docker Engine**:
   - Run `docker builder prune -f` (frees massive build cache layers).
   - Run `docker system prune` (for dangling images and stopped containers) with user consent.

### Step 2: Post-Cleanup Size Confirmation
Measure target directories and report space saved per ecosystem.

---

## Validation Checklist

- [ ] Native package manager CLI commands utilized where possible.
- [ ] No active repository dependencies (`node_modules`, `vendor`) deleted unless specifically scoped.
- [ ] Total space reclaimed tallied accurately.
