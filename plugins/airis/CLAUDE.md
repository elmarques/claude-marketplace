# Airis Plugin

Claude Code plugin for AI-native E2E testing.

Tests describe **what** (user intent), not **how** (selectors). Claude translates, agent-browser executes with visual awareness.

## Development Guidelines

### Commands

- Orchestrators — coordinate test execution, don't implement browser logic
- Load CONFIG.md first — stop early if missing
- Use `AskUserQuestion` for test selection, path confirmation
- Screenshots always to `.airis/screenshots/`
- Naming: `{action}.md` (verb)

### Frontmatter

```yaml
---
description: What it does (shown in command list)
argument-hint: <file> [--flags] # required | optional
---
```

### Execution Flow

```
ATML step → Claude translates → Bash: agent-browser {cmd} → parse result
```

### Testing the Plugin

1. `claude --plugin-dir /path/to/airis`
2. Full flow: setup → create → run --headed
3. Edge cases: missing CONFIG, auth failure, element not found, cleanup failures

## Architecture

```
/airis:setup
         ↓
    creates .airis/ structure
    CONFIG.md with @flows, .env.local template

/airis:create (wizard)
         ↓
    AskUserQuestion: name, path, auth, tests
         ↓
    translates natural language → ATML
         ↓
    writes .airis/tests/{path}

/airis:run <file> [--flags]
         ↓
    loads CONFIG.md (@flows, credentials)
         ↓
    for each test (# Heading):
         ↓
    auth flow → setup → steps → verify
         ↓
    agent-browser executes each command
         ↓
    report results, cleanup E2E_ data
```

## Key Principles

- **Intent over implementation** — tests readable, execution adapts
- **Visual-first** — no brittle selectors
- **Self-contained** — everything in `.airis/`, portable
