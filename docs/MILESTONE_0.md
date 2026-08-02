# Milestone 0 — Desktop Bootstrap

## Goal

Create a clean, runnable Windows-first Aporia Forge desktop shell with a Go backend boundary and a modern static three-panel interface. This milestone does **not** integrate the live DeepSeek API or execute project tools.

## Required stack

- Go
- Wails v2 stable
- React
- TypeScript
- Vite

Use versions compatible with the installed local toolchain. Do not adopt Wails v3 pre-release for this milestone.

## Deliverables

### Desktop foundation

- Initialize a Wails v2 React/TypeScript application in the repository root.
- Use the application title `Aporia Forge`.
- Keep the Go entry point small.
- Add a minimal application service that exposes typed app metadata to the frontend.
- Use generated Wails bindings instead of manual HTTP communication.

### Initial interface

Build a dark, modern, compact three-panel desktop layout:

```text
┌─────────────────────────────────────────────────────────────┐
│ Aporia Forge · workspace · DeepSeek V4 Flash · Local       │
├───────────────┬──────────────────────────────┬──────────────┤
│ Projects      │ Conversation                 │ Changes      │
│ Sessions      │ empty state / mock messages  │ empty state  │
│               │ composer                     │              │
└───────────────┴──────────────────────────────┴──────────────┘
```

Required UI qualities:

- dense but readable desktop proportions
- resizable-feeling panel boundaries, even if resizing is deferred
- no gradients, glassmorphism or oversized marketing cards
- no copied OpenCode branding/assets
- consistent spacing and typography
- clear empty states
- responsive behavior for narrower desktop windows

### State and components

Create small components rather than one monolithic page. At minimum:

- `AppShell`
- `TopBar`
- `Sidebar`
- `ConversationPanel`
- `ChangesPanel`
- `Composer`

Static mock data is allowed only through a clearly named local mock module. Do not mix mock data into component definitions.

### Documentation

Update the README with exact Windows development prerequisites and commands that were actually verified.

## Exclusions

Do not implement:

- DeepSeek API calls
- API key storage
- agent loop
- filesystem access
- command execution
- git integration
- SQLite/session persistence
- Monaco editor
- terminal emulator
- model/provider selectors
- auto updater

## Acceptance criteria

1. `wails doctor` reports a usable Windows development environment, or missing prerequisites are documented honestly.
2. Frontend dependency installation succeeds.
3. Go formatting succeeds.
4. Frontend type checking succeeds.
5. Frontend production build succeeds.
6. `wails build` succeeds on Windows.
7. The built application opens with the three-panel Aporia Forge shell.
8. No secrets, generated binaries, dependency folders or local databases are committed.
9. The final report lists every command run and its actual result.

## Definition of done

Milestone 0 is complete only when the desktop application can be built from a clean checkout using the documented commands and the visual shell is stable enough to become the base for Milestone 1.
