# Command: agent

Runtime entry point for Antigravity IDE latency optimization, subagent frame recordings purge, and file watcher tuning.

## Dispatch steps

1. Load `../workflows/tune-agent.md` — ordered steps for hook latency audits, browser recording purges, and watcher exclusion updates.
2. Load `../memory/agent-tuning.md` — optimal watcher exclude snippets, search exclusion rules, and hook performance standards.
3. Execute Phase 1: Environment and recordings inspection.
4. Do NOT load `../memory/lock-matrix.md` or `../memory/safety-matrix.md`.

## Omissions

This command does NOT mutate agent instructions, persona definitions, or active conversation state.
