# Command: brief

Runtime entry point for full-drive storage diagnostics, disk bottlenecks, and emergency triage.

## Dispatch steps

1. Load `../workflows/diagnose-storage.md` — ordered diagnostic sequence for storage triage.
2. Load `../memory/storage-matrix.md` — standard cache paths, environment variables, and diagnostic commands across Windows, macOS, and Linux.
3. Execute Phase 1: Auto-sensing and rapid measurement.
4. Do NOT load `../memory/lock-matrix.md` or `../memory/safety-matrix.md` during triage.

## Omissions

This command does NOT mutate or delete any files. It purely inspects and presents actionable reclaimable metrics with structured CDL choices.
