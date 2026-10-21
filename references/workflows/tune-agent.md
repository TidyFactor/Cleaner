# Workflow: tune-agent

One outcome: optimized Antigravity IDE agent performance, reduced prompt turn latency, purged ephemeral browser recordings, and tuned file system watchers.

---

## Steps

### Step 0: Context Auto-Sensing
1. Locate Antigravity storage and configuration paths dynamically:
   - Config directory: `~/.gemini/config/`
   - IDE storage: `~/.gemini/antigravity-ide/`
   - Settings files:
     - `$env:APPDATA\Antigravity IDE\User\settings.json`
     - `$env:APPDATA\Antigravity\User\settings.json`

### Step 1: Pre-Invocation Hook Latency Audit
1. Inspect `~/.gemini/config/hooks.json`.
2. Check for any enabled `PreInvocation` hooks executing shell commands:
   - Example: `cmd /c "echo {\"injectSteps\": []}"`
3. If an enabled hook produces empty or no-op inject steps, disable it (`"enabled": false`) to eliminate the process spawn penalty (saving 50-200ms per agent prompt turn).

### Step 2: Ephemeral Browser Recording Purge
1. Inspect `~/.gemini/antigravity-ide/browser_recordings/`.
2. Identify completed past conversation recording directories (excluding the current active conversation ID).
3. Purge accumulated `.jpg` frame sequences to free disk space and eliminate tens of thousands of file table entries.

### Step 3: File Watcher & Search Tuning
1. In `settings.json`, inspect `files.watcherExclude` and `search.exclude`.
2. Ensure high-churn agent directories are explicitly excluded from file system watchers and text search:
   - `**/.gemini/antigravity-ide/browser_recordings/**`
   - `**/.gemini/antigravity-ide/brain/**`
   - `**/.gemini/antigravity-ide/conversations/**`
   - `**/build/**`, `**/dist/**`, `**/.cache/**`, `**/temp/**`
3. Validate JSON syntax integrity after updating settings.

---

## Validation Checklist

- [ ] `hooks.json` validated as strictly valid JSON.
- [ ] `settings.json` validated as strictly valid JSON.
- [ ] Current active conversation recordings untouched; only past sessions cleaned.
- [ ] Disk space gain and watcher improvements verified.
