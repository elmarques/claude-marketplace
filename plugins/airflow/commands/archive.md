---
description: Archive completed plan and merge to base branch
---

# Archive

## 1. Validate

1. Read `.airflow/CONFIG.md` — if missing → suggest `/airflow:setup`
2. Read `.airflow/current/PLAN.md` — if missing → nothing to archive
3. Incomplete `- [ ]` → warn, AskUserQuestion: Continue anyway | Abort
4. Not on `{prefix}{slug}` branch → warn

## 2. Archive

Move PLAN.md to `.airflow/archive/{YYYY-MM-DD}-{slug}.md`

## 3. Merge

AskUserQuestion: Squash (recommended) | Regular | Don't merge

If merge:

1. Checkout `branch.base`
2. Attempt merge
3. Conflict → abort, checkout feature branch, explain manual steps, stop
4. Success → display suggested commit message
5. AskUserQuestion: delete feature branch?

## 4. Cleanup

AskUserQuestion: push to remote?
