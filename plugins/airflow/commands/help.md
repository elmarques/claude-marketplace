---
description: Show Airflow usage guide
---

# Help

Print the following:

```
Airflow — Autonomous coding workflow

SETUP
  /airflow:setup      Initialize .airflow/ in project
  /airflow:help       Show this help
  /airflow:status     Show current workflow state

WORKFLOW
  /airflow:plan       Create PLAN.md with tasks
  /airflow:build      Execute one task (human-in-the-loop)
  /airflow:build-all  Execute all tasks (autonomous)
  /airflow:resume     Resume interrupted build
  /airflow:review     Code review before merge
  /airflow:archive    Archive plan and merge

TYPICAL FLOW
  1. /airflow:setup   (first time only)
  2. Edit .airflow/CONFIG.md
  3. /airflow:plan "feature description"
  4. /clear
  5. /airflow:build-all
  6. /airflow:review
  7. /airflow:archive

CONFIG.md
  ---
  check: <command>       # e.g., npm run check, make lint
  test: <command>        # e.g., npm test, pytest
  model: opus            # or sonnet (default: opus)
  branch:
    prefix: "{type}/"    # template: {type} from PLAN.md (feat/fix/etc)
    base: main           # base branch (default: main)
  ---
  # Rules
  (project-specific guidelines for agents)

FILES
  .airflow/CONFIG.md         Project config and rules
  .airflow/current/PLAN.md   Current work plan
  .airflow/LEARNINGS.md      Gotchas for future tasks
  .airflow/archive/          Completed plans

MORE INFO
  https://github.com/elmarques/claude-marketplace/tree/main/plugins/airflow
```
