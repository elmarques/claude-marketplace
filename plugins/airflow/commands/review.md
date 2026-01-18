---
description: Code review before merge.
argument-hint: "[staged | branch-name] (optional)"
---

# Review

## 1. Setup

**You coordinate only — never review or fix yourself.**

1. Read `.airflow/CONFIG.md` — if missing → suggest `/airflow:setup`
2. Diff mode from argument:
   - `staged` → staged changes only
   - `{branch}` → that branch
   - none → current vs base
3. No changes → inform user, stop

## 2. Review

Spawn: `Task(general-purpose, model from config)` with prompt:

```
Code review. Be wise colleague, not linter.

READ FIRST:
- .airflow/CONFIG.md (branch.base, project rules)
- .airflow/current/PLAN.md (scope context, if exists)
- .airflow/LEARNINGS.md (known gotchas)

DIFF MODE: {mode}
- staged → `git diff --staged`
- {branch} → `git diff {base}...{branch}`
- default → `git diff {base}...HEAD`

SEVERITY:
- P0 Security: auth bypass, injection, data exposure
- P1 Correctness: bugs, race conditions, broken functionality
- P2 Architecture: wrong patterns, tight coupling
- P3 Clarity: naming, minor consistency

CALIBRATION:
- Only flag what you'd reject your own PR for
- Skip preferences — follow project rules
- Max 5-7 issues

RETURN:
APPROVED

OR

CHANGES REQUESTED

#1 [P{n}] {title}
Where: {file:line}
What: {description}
Fix: {suggestion}
```

## 3. Handle Response

**APPROVED** → "Ready to merge." Suggest `/airflow:archive`

**CHANGES REQUESTED:**

1. Display issues
2. AskUserQuestion: Fix all | P0-P2 only | P0-P1 only | Don't fix

## 4. Apply Fixes

Spawn: `Task(general-purpose, model from config)` with prompt:

```
Apply review fixes.

READ FIRST:
- .airflow/CONFIG.md (check command, project rules)
- .airflow/LEARNINGS.md (known gotchas)

ISSUES TO FIX:
{issues}

PROCESS:
1. Apply each fix
2. Run `check` until pass
3. Commit with descriptive message
4. Reusable gotcha → append to LEARNINGS.md

CONSTRAINTS:
- Only what was requested
- Unclear fix → skip and report

RETURN (one line only):
FIXED: {count} issues addressed
PARTIAL: {fixed} fixed, {skipped} skipped — #N reason, #N reason
ERROR: {what failed}
```

## 5. After Fixes

- `FIXED` → AskUserQuestion: Re-review (recommended) | Skip to archive
- `PARTIAL` → show skipped, AskUserQuestion: Re-review (recommended) | Skip to archive
- `ERROR` → show error, AskUserQuestion: Retry | Fix manually | Abort
