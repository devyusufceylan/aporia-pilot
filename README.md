# Aporia Forge

Aporia Forge is a local-first AI coding workspace designed for DeepSeek efficiency and a modern desktop developer experience.

> OpenCode-quality product experience, Reasonix-style DeepSeek efficiency.

## Status

Early development — Phase 0 bootstrap.

## Product principles

- **DeepSeek-first:** the MVP supports only `deepseek-v4-flash`.
- **Cost-aware:** stable prompt prefixes, compact tool results, visible token/cache/cost usage.
- **Local-first:** project files, sessions and command execution stay on the user's machine.
- **Simple by default:** one coding agent, one provider, one clear workflow.
- **Safe execution:** project-scoped filesystem access and explicit permission gates for risky commands.
- **Modern desktop UX:** project/session navigation, chat, tool activity, diff review and terminal output.

## MVP scope

- Windows-first desktop application
- Go backend and agent engine
- Wails v2 desktop shell
- React + TypeScript frontend
- Direct DeepSeek API integration
- Streaming chat
- Project folder selection
- File read/search/edit tools
- Command execution with permissions
- Session persistence
- Diff review and checkpoint/undo
- Cache-hit, token and cost visibility

## Explicit non-goals for MVP

- Multiple model providers
- Multi-agent orchestration
- MCP/plugin marketplace
- Web search
- Cloud accounts or synchronization
- Autonomous git push
- Telemetry that sends source code or prompts off-device

## Development strategy

Development is delivered in small, testable milestones. Every milestone must leave the repository buildable and must include clear acceptance checks.

See:

- [`REASONIX.md`](REASONIX.md) — coding-agent operating rules
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — initial system boundaries
- [`docs/MILESTONE_0.md`](docs/MILESTONE_0.md) — first implementation milestone
