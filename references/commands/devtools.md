# Command: devtools

Runtime entry point for pruning package manager caches (`npm`, `pnpm`, `pip`, `composer`, `cargo`) and orphaned build staging directories.

## Dispatch steps

1. Load `../workflows/clean-devtools.md` — ordered steps for dev package cache verification and cleanup.
2. Load `../memory/storage-matrix.md` — standard cache locations for major package ecosystems across platforms.
3. Execute Phase 1: Tool presence check and cache size calculation.
4. Do NOT load `../memory/safety-matrix.md`.

## Omissions

This command does NOT delete project-local `node_modules` or `vendor` directories unless explicitly requested within a specific repository context.
