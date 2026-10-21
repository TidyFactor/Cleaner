# TidyFactor Cleaner

[![TidyFactor](https://img.shields.io/badge/TidyFactor-Standard-blue.svg)](https://github.com/tidyfactor)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Platform: Cross-Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-orange.svg)]()

> **Universal Developer Environment, IDE & System Hygiene Engine** built on the TidyFactor 14 Structural Rules with Contextual Decision Layer (CDL).

---

## 🎯 Value Proposition

Standard disk cleanup utilities fail developers: they don't understand semantic extension versions, get blocked by zombie background processes, or risk wiping browser credentials and active sessions. 

`tidyfactor-cleaner` provides deterministic, multi-tier hygiene tailored for modern development stacks:
- **Fast Non-Blocking Diagnostics**: Uses Robocopy/`du` summary scans that avoid hanging on deep directory trees.
- **Semantic IDE Version Deduplication**: Automatically detects and prunes obsolete extension folders, resolves child process locks (`codex-code-mode-host`, `kilo`), and clears `.vsix` installer caches.
- **Inviolable Browser Cache Isolation**: Safely extracts and purges gigabytes of pure web, code, GPU, and service worker caches across multi-profile setups without ever risking saved passwords, sessions, or bookmarks.
- **AI Agent Performance Tuning**: Audits hooks to eliminate pre-invocation latency, purges ephemeral browser subagent frame sequences, and hardens file system watcher rules.
- **Universal Framework & Stack Hygiene**: Deterministic cleanup for Next.js, Vite, Nuxt, Laravel, Symfony, WordPress, Docker, and modern package managers (PNPM, Bun, Yarn) using CLI-first orchestration.

---

## ⚡ Commands

| Command | Action | Key Files Loaded |
| :--- | :--- | :--- |
| `brief` / `audit` | Comprehensive storage triage & reclaimable calculation | `diagnose-storage.md` + `storage-matrix.md` |
| `ide` / `vscode` | Prunes obsolete extensions, resolves locks, clears VSIXs | `clean-ide.md` + `lock-matrix.md` |
| `browser` | Safe multi-profile Chromium/Chrome/Edge cache strip | `clean-browser.md` + `safety-matrix.md` |
| `devtools` | Cleans npm, pnpm, bun, pip, composer, cargo, and framework stores | `clean-devtools.md` + `storage-matrix.md` |
| `agent` | Tunes Antigravity hook latency, frame recordings, watchers | `tune-agent.md` + `agent-tuning.md` |

---

## 🛡️ Non-Negotiable Invariants

1. **Credentials & Auth SSOT**: `Login Data`, `Cookies`, `Bookmarks`, and `Preferences` are strictly protected.
2. **Auto-Sensing First**: Zero questions asked about host OS, drive letters, or directory paths.
3. **No Unannounced Deletions**: Clear MB/GB projections and interactive options presented before execution.
4. **Universal Workspace Invariant**: Strictly protects source repositories, `.env` secrets, databases, and local AI weights (`.gguf`, `.safetensors`).

---

## 📄 License
MIT License. Part of the TidyFactor Skill Ecosystem.
