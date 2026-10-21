# Command: browser

Runtime entry point for multi-profile Chromium, Chrome, and Edge browser cache purging while preserving personal databases.

## Dispatch steps

1. Load `../workflows/clean-browser.md` — ordered steps for multi-profile cache isolation, process shutdown, and integrity verification.
2. Load `../memory/safety-matrix.md` — immutable safety invariants and whitelist of preserved databases (`Login Data`, `Cookies`, `Bookmarks`, `Preferences`).
3. Execute Phase 1: Multi-profile discovery and pure cache measurement.
4. Do NOT load `../memory/lock-matrix.md` or `../memory/agent-tuning.md`.

## Omissions

This command strictly excludes passwords, session tokens, browser history, bookmarks, and extension states.
