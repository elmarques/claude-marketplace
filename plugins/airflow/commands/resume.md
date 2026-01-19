---
description: Resume an interrupted build
---

# Resume

## Process

Use after `/clear`, timeout, or any interruption.

1. Read `.airflow/CONFIG.md` — if missing → suggest `/airflow:setup`
2. Read `.airflow/current/PLAN.md` — if missing → suggest `/airflow:plan`
3. Uncommitted changes → AskUserQuestion: Stash | Commit | Continue anyway | Abort
4. Count pending `- [ ]` and completed `- [x]`
5. Check branch matches `{prefix}{slug}` (resolve `{type}`)
6. Show summary, ask to confirm

## Output

```
Plan: {scope}
Progress: {done}/{total}
Next: {task description}
```

AskUserQuestion: Continue build (recommended) | Abort

## On Confirmation

Run `/airflow:build-all` loop from first unchecked task.

## Edge Cases

- All complete → "All done." Suggest `/airflow:review`
- Branch mismatch → AskUserQuestion: Switch to correct branch (recommended) | Stay on current | Abort
