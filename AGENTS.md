# AGENTS.md — TidyFactor Cleaner Skill Governance

This file governs the behavior of all AI Coding Agents (*Google Antigravity, Claude Code, Cursor, Codex, Windsurf*) working within the `tidyfactor-cleaner` skill.

---

## 🏛️ Non-Negotiable Governance Rules

1. **Dispatcher Discipline**: `SKILL.md` is strictly a router (~350 tokens). All domain logic and workflows live under `references/`.
2. **One Workflow = One Outcome**: Every workflow in `references/workflows/` ends with a mandatory `## Validation checklist`.
3. **Operational Memory**: All memory files under `references/memory/` contain pure facts, schemas, and catalogs with `<!-- last-verified: 2026-10-21 -->`.
4. **Safety Invariants**:
   - Strictly forbidden from deleting or mutating user authentication, session, or credential databases (`Login Data`, `Cookies`, `Bookmarks`, `Preferences`).
   - Strictly forbidden from deleting local AI weights (`*.gguf`, `*.safetensors`), environment secrets (`.env*`), active source trees, or databases.
   - Dependency stores (`node_modules`, `.venv`) may only be pruned when verified backing lockfiles exist.
5. **Auto-Sensing First (Rule 14)**: Always discover host operating system, environment variables, and drive letters dynamically before asking the user.
6. **No Unannounced Deletions**: Always calculate exact reclaimable megabytes and present structured multiple-choice options (A/B/C/D) before performing file removals.
7. **Lock Resolution Discipline**: If a file or directory is locked by an active language server or daemon (e.g. `gopls.exe`, `tsserver`), gracefully report PID and handle release options.
