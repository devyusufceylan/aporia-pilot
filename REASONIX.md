# Aporia Forge — Reasonix working contract

This file is the operating contract for every Reasonix session in this repository.

## Product goal

Build a modern local-first desktop coding agent that combines:

- Reasonix-style DeepSeek efficiency
- OpenCode-quality desktop UX
- a deliberately small and understandable feature set

The product name is **Aporia Forge**.

## Fixed MVP decisions

- Backend and agent engine: **Go**
- Desktop framework: **Wails v2 stable**
- Frontend: **React + TypeScript + Vite**
- Primary platform: **Windows**
- Model: **`deepseek-v4-flash` only**
- Provider: direct DeepSeek API connection
- Default workflow: one coding agent, no subagents
- Data: local sessions and settings

Do not replace these choices without an explicit user instruction.

## Scope discipline

Implement only the active milestone. Do not opportunistically add unrelated features.

Do not add the following during the MVP:

- extra model providers
- automatic Flash-to-Pro escalation
- multi-agent orchestration
- MCP or plugin systems
- web search or browser automation
- cloud authentication or synchronization
- analytics/telemetry that leaves the machine
- autonomous `git push`, destructive git commands or filesystem access outside the workspace

## Architecture rules

1. The Go engine must not depend on React, Wails UI components or frontend state.
2. DeepSeek API code must live behind a small provider interface.
3. Tool execution must be workspace-scoped and permission-aware.
4. UI communication must use typed events and typed request/response models.
5. Session history must be append-oriented so a future cache-stable context loop can be implemented without rewriting the storage layer.
6. Long command/tool output must be bounded before it reaches the model.
7. Never store API keys in the repository, project `.env`, logs, sessions or test fixtures.
8. Prefer standard-library Go packages unless a dependency has a clear, documented benefit.

## Working method

Before editing:

1. Read this file and the active milestone document.
2. Inspect the existing repository structure.
3. State the files that will change and the acceptance checks.
4. Use a plan only for multi-file or architectural work.

While editing:

- Keep changes small and reviewable.
- Do not rewrite unrelated files.
- Prefer patches over full-file rewrites.
- Add tests for backend behavior that is not purely visual.
- Preserve buildability after every completed task.

After editing:

1. Format Go and frontend code.
2. Run the narrowest relevant tests first.
3. Run the milestone acceptance commands.
4. Report changed files, test results and known limitations.
5. Do not claim success when a command was not run or failed.

## Git rules

- Never commit secrets, databases, generated builds, caches, reports or dependency folders.
- Never run `git push`, `git reset --hard`, history rewriting or branch deletion without explicit approval.
- Use focused conventional commits.
- One milestone should normally be delivered through one branch and one pull request.

## DeepSeek efficiency rules

The future agent loop must preserve these invariants:

- immutable system/tool prefix within a session
- append-only conversation/event history
- volatile per-turn scratch kept outside the stable prefix
- minimal tool schemas
- bounded tool output
- no timestamps, random IDs or reordered content in the stable prompt prefix
- visible token, cache-hit and cost accounting

Do not imitate Reasonix code blindly. Reimplement only the concepts required by Aporia Forge and retain all required third-party license notices when source code is reused.
