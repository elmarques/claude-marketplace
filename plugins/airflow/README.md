# Airflow

Autonomous coding workflows for Claude Code.

## Why?

Long coding sessions accumulate context. LLMs start forgetting early decisions, hallucinating file paths, mixing up variable names (context rot). Airflow solves this with the [Ralph Wiggum technique](https://ghuntley.com/ralph/):

- **Fresh context per task** — each task spawns a new agent with clean slate
- **Plan as persistent memory** — PLAN.md carries decisions across tasks
- **Learnings survive** — gotchas discovered are saved for future sessions

Result: consistent output even on 20+ task sessions.

## Installation

### Via Marketplace (recommended)

```bash
/plugin marketplace add elmarques/claude-marketplace
/plugin install airflow@elmarques-marketplace
```

Then enable auto-updates to receive new versions automatically:

```
/plugin → Marketplaces → elmarques-marketplace → Enable auto-update
```

### Local Development

```bash
git clone https://github.com/elmarques/claude-marketplace.git
claude --plugin-dir ./claude-marketplace/plugins/airflow
```

## Quick Start

```bash
/airflow:setup                        # Initialize in your project
# Edit .airflow/CONFIG.md with your check command and any project rules
/airflow:plan "Add user authentication"
/airflow:build-all                    # Autonomous execution
/airflow:review                       # Code review
/airflow:archive                      # Merge and archive
```

## Commands

| Command              | Description                        |
| -------------------- | ---------------------------------- |
| `/airflow:setup`     | Initialize `.airflow/` in project  |
| `/airflow:plan`      | Create PLAN.md with tasks          |
| `/airflow:build`     | Execute one task (human oversight) |
| `/airflow:build-all` | Execute all tasks (autonomous)     |
| `/airflow:review`    | Code review before merge           |
| `/airflow:archive`   | Archive plan and merge             |
| `/airflow:status`    | Show workflow state                |
| `/airflow:resume`    | Resume interrupted build           |
| `/airflow:help`      | Show usage guide                   |

## How It Works

```
/airflow:build-all
       ↓
  for each task:
       ↓
    spawn fresh agent → implement → DONE/BLOCKED/ERROR
       ↓
  orchestrator continues or handles
```

Each agent reads CONFIG.md (rules), PLAN.md (tasks), and LEARNINGS.md (gotchas) — then executes one task and exits. No context bleed between tasks.

## Configuration

After `/airflow:setup`, edit `.airflow/CONFIG.md`:

```yaml
---
check: <command> # e.g., npm run check, make lint, cargo check
test: <command> # e.g., npm test, pytest, go test (optional)
model: opus # opus (default) or sonnet
branch:
  prefix: airflow/
  base: main
---
```

The body of CONFIG.md is free-form — add project-specific context for agents, or reference your CLAUDE.md:

```markdown
# Rules

Follow CLAUDE.md. Additional notes for Airflow agents:

- Run tests before marking tasks done
- Prefer small, focused commits
```

## Directory Structure

```
.airflow/
├── CONFIG.md      # Config + project rules
├── LEARNINGS.md   # Gotchas for future tasks
├── current/
│   └── PLAN.md    # Active plan
└── archive/       # Completed plans
```

## License

MIT
