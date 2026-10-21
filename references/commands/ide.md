# Command: ide

Runtime entry point for cleaning VS Code, Antigravity, and Cursor extension stores, obsolete versions, and zombie process locks.

## Dispatch steps

1. Load `../workflows/clean-ide.md` — the step sequence for semantic version grouping, lock inspection, and obsolete pruning.
2. Load `../memory/lock-matrix.md` — known background daemon names (`codex`, `kilo`, language servers) and termination commands.
3. Execute Phase 1: Semantic extension version discovery.
4. Do NOT load `../memory/safety-matrix.md` (browser data).

## Omissions

This command does NOT touch active user settings, keybindings, snippets, or current active extension versions.
