---
description: Create work plan for Airflow
argument-hint: [scope description]
context: fork
---

# Plan

## 1. Load

1. Read `.airflow/CONFIG.md` — if missing → suggest `/airflow:setup`
2. Note project rules

## 2. Understand Scope

1. No argument → AskUserQuestion: "What do you want to build?" (free text input)
2. PLAN.md exists → AskUserQuestion: Overwrite | Abort
3. Study codebase — patterns, conventions, dependencies
4. Consider project rules
5. Clarify with AskUserQuestion if needed

## Writing Style

Concise markdown. Every token counts.

- Drop subjects/articles — `Read file` not `You should read the file`
- Symbols — `→` (result), `|` (or), `CAPS:` (attention)
- Imperatives, lists > prose

## 3. Create Plan

Write `.airflow/current/PLAN.md`:

```markdown
---
type: feat
slug: { url-safe-slug }
---

# [Scope Name]

One paragraph: what we're building.

## Guidelines

- Key decisions
- Patterns to follow

## Out of Scope

- What NOT to build

## Tasks

- [ ] {slug} — Description. **Done when:** observable criteria

## Blockers

(none)

## Notes

- YYYY-MM-DD: Plan created
```

## Task Rules

- **Unique slug** — lowercase, hyphens
- **One outcome** — two things to test = two tasks
- **Observable criteria** — not "works" but "returns X"
- **Dependency order** — foundations first
- **Bias small** — easier to verify and recover
- **Commit test** — wouldn't make sense as standalone commit = wrong size

## Good vs Bad "Done When"

**Bad:** "Works", "Complete", "Fixed", "Implemented" — vague, unverifiable

**Good:**

- "GET /api/users returns array with id, name, email"
- "Form submits and redirects to /dashboard"
- "Test passes: `{test command} path/to/test`"
- "Login button visible, clicking opens modal"

## Types

For `{type}` in branch prefix: `feat` | `fix` | `refactor` | `chore` | `docs` | `test`

## Slugs

URL-safe (lowercase, hyphens):

- Plan: "User Authentication" → `user-authentication`
- Task: "Create login form" → `create-login-form`

## 4. Confirm

Present summary. AskUserQuestion: Approve plan (recommended) | Adjust tasks | Adjust scope | Start over

## Output

```
Plan: {slug} ({type})
Tasks: {n}
Branch: {resolved prefix}{slug}
```

Suggest `/clear` then `/airflow:build-all`, or `/airflow:build` for human-in-the-loop.
