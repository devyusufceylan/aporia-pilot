# Aporia Forge — Initial Architecture

## System shape

```text
┌──────────────────────────────────────────────────────┐
│ Desktop UI — React + TypeScript                      │
│ projects · sessions · chat · activity · diff · cost │
└──────────────────────────┬───────────────────────────┘
                           │ typed Wails bindings/events
┌──────────────────────────▼───────────────────────────┐
│ Application layer — Go                              │
│ commands · orchestration · lifecycle · permissions  │
└──────────────────────────┬───────────────────────────┘
                           │ interfaces
┌──────────────────────────▼───────────────────────────┐
│ Forge engine — Go                                    │
│ agent loop · context · tools · sessions · checkpoints│
└──────────────┬───────────────────────┬───────────────┘
               │                       │
┌──────────────▼─────────────┐  ┌──────▼──────────────┐
│ DeepSeek provider          │  │ Local capabilities  │
│ streaming · usage · errors │  │ files · shell · git │
└────────────────────────────┘  └─────────────────────┘
```

## Initial package boundaries

```text
cmd/forge/                    application entry point
internal/app/                 Wails-facing application services
internal/agent/               model/tool iteration loop
internal/context/             stable-prefix and turn-context construction
internal/provider/            model provider contracts
internal/provider/deepseek/   DeepSeek HTTP/SSE implementation
internal/session/             append-only session/event persistence
internal/tools/               tool registry and implementations
internal/security/            workspace boundaries and command permissions
internal/checkpoint/          changed-file snapshots and restoration
internal/usage/               token/cache/cost accounting
frontend/src/                 React application
```

These are target boundaries, not a requirement to create every package during bootstrap.

## Core contracts

### Provider

The provider is responsible only for model communication:

- accept a normalized request
- stream normalized response events
- expose usage fields returned by DeepSeek
- convert transport/API failures into typed errors

It must not read project files, execute tools or own session history.

### Agent engine

The agent engine owns the iterative flow:

1. build context
2. request a streamed model response
3. collect assistant output or tool calls
4. validate and dispatch permitted tools
5. append model/tool events to the session
6. repeat until completion or a configured iteration limit

### Session store

Sessions are represented as append-only events. Projections may derive chat messages, tool activity and usage totals, but old events are not silently rewritten.

### Tools

The MVP tool surface is intentionally small:

- `read_file`
- `search_files`
- `search_content`
- `apply_patch`
- `write_file`
- `run_command`
- read-only git status/diff operations

Every filesystem path is resolved and verified against the active workspace root.

### UI boundary

The frontend renders state and sends user intent. It must not contain API keys, direct DeepSeek calls, command execution or filesystem security logic.

## Cache-stability design

A future request is constructed from three regions:

```text
IMMUTABLE PREFIX
  product/system policy
  ordered tool specifications
  stable project instructions

APPEND-ONLY SESSION LOG
  user messages
  assistant messages
  tool calls and bounded results

VOLATILE TURN SCRATCH
  current UI attachments
  transient approvals
  per-turn instructions
```

The immutable prefix must remain byte-stable for the life of a session. Dynamic timestamps, random identifiers and unordered maps must never be serialized into it.

## Security baseline

- Default filesystem scope is the selected workspace.
- Reads outside the workspace are denied unless explicitly allowlisted.
- Writes outside the workspace are denied.
- Destructive or network-affecting commands require approval.
- API credentials are loaded from secure local configuration and redacted from logs.
- No source code, prompts or tool output are sent anywhere except the configured DeepSeek endpoint.

## MVP delivery order

1. Desktop shell and typed Go/frontend bridge
2. Workspace and local session foundations
3. Direct DeepSeek streaming chat
4. Read/search tools
5. Patch/write and permission gates
6. Command execution
7. Diff and checkpoint/undo
8. Cache-stable agent loop and usage visibility
