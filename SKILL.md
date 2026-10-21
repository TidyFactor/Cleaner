---
name: tidyfactor-cleaner
description: "Universal developer environment, IDE, framework (Next.js, Laravel, Docker), and system hygiene engine with Contextual Decision Layer (CDL). Safely reclaims disk space, deduplicates VS Code/Antigravity extensions, purges multi-profile browser caches without losing logins, tunes agent latency, and eliminates zombie process locks across Windows, macOS, and Linux. Trigger on commands 'brief', 'ide', 'browser', 'devtools', 'agent', 'clean', 'hygiene', 'storage', or requests to free disk space."
---

# TidyFactor Cleaner

A command dispatcher for developer workstation, IDE, and system hygiene. Conforms to the TidyFactor 14 Structural Rules. Every command routes to a single workflow and loads its dedicated operational memory.

## Commands

| User intent | Command file | What it loads |
| :--- | :--- | :--- |
| "Scan disk space" / "Where is storage going?" / "Triage disk" | `references/commands/brief.md` | `workflows/diagnose-storage.md` + `memory/storage-matrix.md` |
| "Clean VS Code" / "Remove old extensions" / "Free IDE space" | `references/commands/ide.md` | `workflows/clean-ide.md` + `memory/lock-matrix.md` |
| "Clean Chrome cache" / "Browser cleanup without losing logins" | `references/commands/browser.md` | `workflows/clean-browser.md` + `memory/safety-matrix.md` |
| "Clean npm / pip / composer" / "Prune dev package caches" / "Clean Laravel / Next.js" | `references/commands/devtools.md` | `workflows/clean-devtools.md` + `memory/storage-matrix.md` |
| "Optimize Antigravity" / "Speed up agent" / "Clean recordings" | `references/commands/agent.md` | `workflows/tune-agent.md` + `memory/agent-tuning.md` |

## Non-negotiable constraints

1. **Safety Invariant**: Strictly forbidden from deleting or mutating user authentication, session, or credential databases (`Login Data`, `Cookies`, `Bookmarks`, `Preferences`).
2. **Auto-Sensing First (Rule 14)**: Always discover operating system, environment variables (`$HOME`, `$env:LOCALAPPDATA`, etc.), and drive letters automatically before asking the user anything.
3. **No Unannounced Deletions**: Always calculate exact reclaimable megabytes and present structured multiple-choice options (A/B/C/D) before performing file removals.
4. **Lock Resolution Discipline**: If a directory is locked by a running process, detect the PID and report it to the user with graceful/terminate options rather than failing silently.
5. **Universal Workspace Invariant**: Strictly forbidden from deleting local AI weights (`*.gguf`, `*.safetensors`), environment secrets (`.env*`), active source trees, or databases. Dependency stores (`node_modules`, `.venv`) may only be pruned when verified backing lockfiles exist.

## Tooling Scope (Rule 10)

- **Languages**: PowerShell (Windows), Bash/POSIX Shell (Linux/macOS), Python
- **Mutations**: Temporary cache and obsolete file pruning (read-only diagnostics by default until confirmed)
- **Network**: None required
