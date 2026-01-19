# Airis

> AI-native test runner. Markdown tests executed via agent-browser.

## The Problem

LLMs write brittle Playwright tests — guessing selectors blind means slow debugging cycles.

## The Solution

Tests describe **what** (user intent), not **how** (selectors). Claude translates, agent-browser executes with visual awareness.

```
Human intent (markdown) → Claude (translator) → agent-browser (executor)
```

## Install

```bash
# Requires agent-browser CLI
npm install -g agent-browser
agent-browser install

# Add plugin to Claude Code
/plugin marketplace add elmarques/claude-marketplace
/plugin install airis@elmarques-marketplace
```

Enable auto-updates:

```
/plugin → Marketplaces → elmarques-marketplace → Enable auto-update
```

## Quick Start

```
/airis:setup                 # Initialize .airis/
/airis:create                # Wizard to create tests
/airis:run file.md --headed  # Run with visible browser
```

## ATML Test Format (markdown)

File: `.airis/tests/academy/courses-crud.md`

```yaml
---
title: Courses CRUD
auth: admin
---

# Create course

## Steps

1. Navigate to /academy/courses
2. Click button "New Course"
3. Fill "Title" with "E2E_Course_{timestamp}"
4. Click button "Create"

## Verify

- Toast contains "Course created"
- Table has row "E2E_Course_"
```

## With Airflow

1. `/airflow:build` — implement feature
2. `/airis:create` — create test
3. `/airis:run file.md` — verify
4. Mark task done

## Philosophy

> Let the AI figure out _how_. You define _what_.

## Requirements

- Claude Code
- agent-browser CLI
- Node.js 18+

## License

MIT
