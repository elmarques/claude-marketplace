---
description: Show current Airflow workflow state
---

# Status

## Process

1. Check if `.airflow/` exists — if missing → suggest `/airflow:setup`
2. Read `.airflow/CONFIG.md` — show config summary
3. Check for `.airflow/current/PLAN.md`:
   - If exists: show scope, task progress, blockers
   - If not: "No active plan"
4. Get current git branch
5. Check for uncommitted changes
6. Count archived plans
7. Count learnings in LEARNINGS.md

## Output Format

```
Airflow Status
==============

Config: .airflow/CONFIG.md
  Check: {check command}
  Test: {test command or "—"}
  Model: {model}
  Branch: {prefix template} → {base}

Plan: {scope name or "None"}
  Tasks: {completed}/{total}     ← omit if no plan
  Blockers: {count or "None"}    ← omit if no plan

Git:
  Branch: {current branch}
  Changes: {clean | X uncommitted files}

Archive: {count} completed plans
Learnings: {count} entries
```

If no plan → Suggest `/airflow:plan "description"`

If not initialized → `Airflow not initialized.` Suggest `/airflow:setup`
