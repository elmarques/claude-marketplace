---
description: Initialize Airis in current project
model: haiku
---

# Setup

## Prerequisites

`agent-browser` CLI required → if missing, suggest:

```
npm install -g agent-browser && agent-browser install
```

Stop until installed.

## Process

1. Not git repo → warn, AskUserQuestion: Continue anyway | Abort
2. `.airis/` exists → AskUserQuestion: Overwrite | Keep and abort
3. Create structure
4. Report created files

## Structure

```
.airis/
├── CONFIG.md
├── .env.local
├── .gitignore
├── tests/
└── screenshots/
```

## CONFIG.md Template

```markdown
---
baseUrl: http://localhost:PORT
headed: false
timeout: 30000
cleanup_prefix: "E2E_"
---

# Airis Config

Project-specific context for test execution.

## Settings

- `headed` — show browser (default: false)
- `timeout` — max wait ms
- `cleanup_prefix` — test data identifier

Precedence: `--flag` > test frontmatter > CONFIG.md

## Variables

Expanded by Claude before execution:

- `{timestamp}` → Unix ms
- `{uuid}` → random UUID
- `{AIRIS_*}` → env var from `.env.local`

## Authentication

Credentials in `.airis/.env.local` or project `.env`.

### @login-admin

1. Navigate to /auth/login
2. Fill "Email" with {AIRIS_ADMIN_EMAIL}
3. Fill "Password" with {AIRIS_ADMIN_PASSWORD}
4. Click button "Sign in"
5. Wait for URL contains /dashboard

### @login-user

1. Navigate to /auth/login
2. Fill "Email" with {AIRIS_USER_EMAIL}
3. Fill "Password" with {AIRIS_USER_PASSWORD}
4. Click button "Sign in"
5. Wait for URL contains /dashboard

## App Context

Describe app for Claude inference:

- Main sections, navigation
- CRUD patterns
- Where test data lives

## Custom Flows

Reusable flows with `@name {args}`:

### @example-flow {param}

1. Step using {param}
2. Another step
```

## .env.local Template

```
AIRIS_ADMIN_EMAIL=
AIRIS_ADMIN_PASSWORD=
AIRIS_USER_EMAIL=
AIRIS_USER_PASSWORD=
```

## .gitignore Template

```
.env.local
screenshots/
```

## Output

```
Airis initialized.

Created:
- .airis/CONFIG.md
- .airis/.env.local
- .airis/.gitignore
- .airis/tests/
- .airis/screenshots/

Next:
1. Edit CONFIG.md — set baseUrl, auth flows, patterns
2. Add credentials to .env.local
3. Create first test: /airis:create
```
