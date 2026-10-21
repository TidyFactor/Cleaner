# Agent Tuning Matrix: Latency, Watchers & Cache Hygiene

<!-- last-verified: 2026-09-05 -->

Optimization parameters for Antigravity IDE and AI coding agent environments.

## File Watcher & Search Tuning Patterns

Inject into IDE `settings.json`:

```json
{
  "files.watcherExclude": {
    "**/.git/objects/**": true,
    "**/.git/subtree-cache/**": true,
    "**/node_modules/**": true,
    "**/vendor/**": true,
    "**/dist/**": true,
    "**/.cache/**": true,
    "**/.gemini/antigravity-ide/browser_recordings/**": true,
    "**/.gemini/antigravity-ide/brain/**": true,
    "**/.gemini/antigravity-ide/conversations/**": true,
    "**/build/**": true,
    "**/temp/**": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/vendor": true,
    "**/dist": true,
    "**/build": true,
    "**/*.min.js": true,
    "**/*.min.css": true,
    "**/.gemini/antigravity-ide/browser_recordings": true,
    "**/.gemini/antigravity-ide/conversations": true
  }
}
```

---

## Pre-Invocation Hook Latency Benchmarks

| Hook Type | Implementation Pattern | Latency Cost per Turn | Recommendation |
| :--- | :--- | :--- | :--- |
| **Spawning Shell (`cmd.exe`)** | `cmd /c "echo ..."` | **+50ms to 200ms** | **Disable** (`"enabled": false`) if empty |
| **Spawning Bash** | `bash -c "echo ..."` | **+30ms to 120ms** | Avoid on every turn |
| **In-Process / Native JS** | Direct memory evaluation | **< 1ms** | Optimal |

---

## Browser Recording Frame Policy
Browser subagents accumulate thousands of WebP / JPEG frame sequence files in `~/.gemini/antigravity-ide/browser_recordings/<conversation-id>/`.
- Active conversation: **Retain** for user reference and replay.
- Past completed conversations: **Safe to purge** periodically to prevent SSD file table bloat.
