---
description: Initialize Airflow in current project
---

# Setup

## Process

1. Not git repo → warn, AskUserQuestion: Continue anyway | Abort
2. `.airflow/` exists → AskUserQuestion: Overwrite | Abort
3. Create structure
4. Report created files

## Structure

```
.airflow/
├── CONFIG.md
├── current/
├── archive/
└── LEARNINGS.md
```

## CONFIG.md Template

```markdown
---
check: <your-check-command> # e.g., npm run check, make lint, cargo check
test: <your-test-command> # e.g., npm test, pytest, go test
model: opus
branch:
  prefix: "{type}/"
  base: main
---

# Rules

Project-specific context for Airflow agents.

Add anything agents should know that isn't in CLAUDE.md,
or just: "Follow CLAUDE.md rules."
```

**Frontmatter fields:**

| Field           | Required | Default     | Description                             |
| --------------- | -------- | ----------- | --------------------------------------- |
| `check`         | Yes      | —           | Fast verification (format, lint, types) |
| `test`          | No       | —           | Heavier tests (unit, e2e)               |
| `model`         | No       | `opus`      | Agent model (`opus`/`sonnet`)           |
| `branch.prefix` | No       | `"{type}/"` | `{type}` from PLAN.md (feat/fix/etc)    |
| `branch.base`   | No       | `main`      | Merge target                            |

**Body:** Free-form. Rules, patterns, or reference to CLAUDE.md.

## LEARNINGS.md Template

```markdown
# Learnings

Gotchas for future tasks. Add if: undocumented + caused time loss + reusable.
```

## Output

```
Airflow initialized.

Created: CONFIG.md, current/, archive/, LEARNINGS.md

Suggest edit CONFIG.md, then `/airflow:plan`
```
