# Airflow Plugin

Claude Code plugin for autonomous coding workflows.

Implements the [Ralph Wiggum technique](https://ghuntley.com/ralph/) — fresh context per task, plan as persistent memory.

## Development Guidelines

### Commands

- Orchestrators, not implementers — coordinate, don't do work
- Validate early — check CONFIG.md/PLAN.md first
- Use `suggest /airflow:cmd` for recommendations to user
- Use `AskUserQuestion: Option A | Option B` for decisions
- Naming: `{action}.md` (verb)

### Frontmatter

```yaml
---
description: What it does (shown in command list)
argument-hint: [optional args] # if command takes arguments
context: fork # isolate context (use for heavy commands like plan)
---
```

### Spawning Tasks

Commands spawn `Task(general-purpose, model)` with **inline prompts**.

```
{role description}

READ FIRST:
- .airflow/CONFIG.md (...)
- .airflow/current/PLAN.md (...)
- .airflow/LEARNINGS.md (...)

PROCESS:
1. ...

CONSTRAINTS:
- ...

RETURN (one line only):
DONE/BLOCKED/ERROR: {details}
```

### Testing

1. `claude --plugin-dir /path/to/airflow`
2. Full flow: setup → plan → /clear → build-all → review → archive
3. Edge cases: no tasks, check fails, interrupted build, no changes, incomplete archive

## Architecture

```
/airflow:plan (context: fork)
         ↓
    analyzes codebase, creates PLAN.md
         ↓
    returns to clean orchestrator context

/airflow:build-all (orchestrator)
         ↓
    spawns Task(general-purpose) with inline prompt
         ↓
    agent reads CONFIG + PLAN + LEARNINGS
         ↓
    implements, returns DONE/BLOCKED/ERROR
         ↓
    orchestrator continues or handles
```

## Key Principle

**Intelligence in prompts, coordination in commands.**

Fresh context each task. Plan as persistent memory.
