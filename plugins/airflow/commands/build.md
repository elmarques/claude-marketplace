---
description: Execute one task from plan with human oversight.
---

# Build

## 1. Load

**You implement directly** (unlike build-all which spawns tasks).

1. Read `.airflow/CONFIG.md` — if missing → suggest `/airflow:setup`
   - Get `check` command — if missing → suggest configure it
   - Note project rules
2. Read `.airflow/current/PLAN.md` — if missing → suggest `/airflow:plan`
3. Uncommitted changes → AskUserQuestion: Stash | Commit | Continue anyway | Abort

## 2. Branch

Read `type` and `slug` from PLAN.md. Switch to `{prefix}{slug}` — resolve `{type}` (create or reuse).

## 3. Execute

1. Find first `- [ ]` task
2. Show task — AskUserQuestion: Start task (recommended) | Skip | Abort
3. Implement per "Done when" + project rules
4. Run `check` until pass
5. Mark `- [x]`, add note: `{date}: {slug} — summary`
6. Commit

## Constraints

- One task only
- Wait for user approval before next
- Only what task needs
- Check fails → never mark done

## On Completion

Report: what done, files changed, commit hash, next task preview.

Reusable gotcha → add to LEARNINGS.md.

## If Blocked

Stop. Explain blocker. Update PLAN.md Blockers section.
