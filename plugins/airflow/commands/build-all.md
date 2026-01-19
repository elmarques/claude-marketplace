---
description: Autonomous build loop using developer agent
---

# Build All

## Rules

- **You coordinate only — never implement**
- Never read code — only PLAN.md to count tasks
- One agent per task — fresh context each time
- No concurrent builds on same plan

## 1. Setup

1. Read `.airflow/CONFIG.md` — if missing → suggest `/airflow:setup`
   - `check` command required — if missing → suggest configure it
   - Get `model` (default: opus), `branch` settings
2. Read `.airflow/current/PLAN.md` — if missing → suggest `/airflow:plan`
3. Uncommitted changes → AskUserQuestion: Stash | Commit | Continue anyway | Abort
4. Count pending `- [ ]` tasks
5. Switch to branch `{prefix}{slug}` (create if needed) — resolve `{type}`
6. Tell user: "Starting build: X tasks pending"

## 2. Loop

For each pending task:

1. Tell user: "Task {n}/{total} [{slug}]: {description}..."
2. Spawn: `Task(general-purpose, model from config)` with prompt:

```
Implement ONE task from PLAN.md.

READ FIRST:
- .airflow/CONFIG.md (check command, project rules)
- .airflow/current/PLAN.md (find first `- [ ]` task)
- .airflow/LEARNINGS.md (known gotchas)

PROCESS:
1. Implement per "Done when" criteria + project rules
2. Run `check` command until pass
3. Mark task `- [x]`, add note: `YYYY-MM-DD: {slug} — {summary}`
4. Commit with descriptive message

WRITING STYLE (for PLAN.md and LEARNINGS.md):
- Concise — drop subjects/articles
- Symbols — `→` (result), `|` (or), `CAPS:` (attention)
- Imperatives, lists > prose

CONSTRAINTS:
- One task only — stop after completing it
- Only what task needs — no extras
- Check fails → never mark done
- Wrong branch → return BLOCKED
- Reusable gotcha found → append to LEARNINGS.md

RETURN (one line only):
DONE: {summary}
BLOCKED: {reason}
ERROR: {what failed}
```

3. Parse response:
   - `DONE` → log, continue
   - `BLOCKED` → AskUserQuestion: Continue to next task | Stop build
   - `ERROR` → AskUserQuestion: Retry task | Stop build
   - Malformed → retry once

## 3. Completion

1. `test` configured → AskUserQuestion: Run full test suite (recommended) | Skip tests
2. Report: "Build complete: {n} tasks." Suggest `/airflow:review`

## Notes

- Each agent gets fresh context (no bleed between tasks)
- Context large → suggest `/clear` then `/airflow:resume`
