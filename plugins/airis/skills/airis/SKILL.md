---
name: airis
description: Write and run AI-native E2E tests using ATML (markdown) and agent-browser. Use when user needs to create tests, run tests, verify UI flows, or work with .airis/ test files.
---

# Airis

AI-native test runner. Tests describe **what** (user intent), not **how** (selectors). Execution via agent-browser.

## When to Use

- User asks to write/add E2E test
- User mentions `.airis/` or ATML
- After implementing a feature → suggest testing

## Commands

| Command                | Description                     |
| ---------------------- | ------------------------------- |
| `/airis:setup`         | Initialize `.airis/` in project |
| `/airis:create [name]` | Wizard to create test           |
| `/airis:run <file>`    | Execute tests                   |
| `/airis:help`          | Usage guide                     |

## Project Structure

```
.airis/
├── CONFIG.md       # Settings, auth flows, patterns
├── .env.local      # Credentials (gitignored)
├── .gitignore
├── tests/          # ATML test files
└── screenshots/    # Failure screenshots (gitignored)
```

---

## ATML Format

```yaml
---
title: Suite Name
auth: admin
headed: true
---

# Test name

## Setup
- @create-data "arg"

## Steps
1. Step one
2. Step two

## Verify
- Assertion one
- Assertion two
```

**Frontmatter:** `title` (report), `auth` (`@login-{value}` once, not in Setup), `headed` (`--headed` wins)

**Sections:** `# Heading` = test. `## Setup` optional. `## Steps` + `## Verify` required.

---

## Step Vocabulary

### Navigation

| Pattern             | Notes                 |
| ------------------- | --------------------- |
| `Navigate to /path` | Also: `Go to`, `Open` |
| `Refresh page`      |                       |
| `Go back`           |                       |

### Interactions

| Pattern                 | Notes               |
| ----------------------- | ------------------- |
| `Click button "X"`      | Button by text      |
| `Click link "X"`        | Link by text        |
| `Click "X"`             | Any clickable       |
| `In row "X", click "Y"` | Scoped to table row |
| `In modal, click "Y"`   | Scoped to dialog    |

### Forms

| Pattern                        | Notes                 |
| ------------------------------ | --------------------- |
| `Fill "Label" with "value"`    | Clears first          |
| `Clear "Label"`                |                       |
| `Type "text"`                  | Appends (no clear)    |
| `Press Enter`                  | Also: `Tab`, `Escape` |
| `Select "Option" from "Label"` | Dropdown              |
| `Check "Label"`                | Checkbox on           |
| `Uncheck "Label"`              | Checkbox off          |

### Waiting

| Pattern                     | When               |
| --------------------------- | ------------------ |
| `Wait for "text"`           | Until text appears |
| `Wait for URL contains "X"` | After navigation   |
| `Wait for page load`        | Heavy actions      |

---

## Assertion Vocabulary

| Pattern                   | Meaning          |
| ------------------------- | ---------------- |
| `Toast contains "X"`      | Toast message    |
| `Page shows "X"`          | Text visible     |
| `Page does not show "X"`  | Text not visible |
| `Table has row "X"`       | Row exists       |
| `Table has no row "X"`    | Row absent       |
| `Table has N rows`        | Count            |
| `URL is /path`            | Exact match      |
| `URL contains "X"`        | Partial match    |
| `Button "X" is disabled`  | State check      |
| `Field "X" has value "Y"` | Input value      |

---

## Variables

Expanded by Claude: `{timestamp}`, `{uuid}`, `{AIRIS_*}` (from `.env.local`)

---

## Flows

Define in `.airis/CONFIG.md`, use in `## Setup`:

```markdown
### @login-admin

1. Navigate to /auth/login
2. Fill "Email" with {AIRIS_ADMIN_EMAIL}
3. Fill "Password" with {AIRIS_ADMIN_PASSWORD}
4. Click button "Sign in"
5. Wait for URL contains /dashboard
```

Use: `- @login-admin` | `- @create-course "E2E_{timestamp}"`

---

## Conventions

| Rule                         | Example                                  |
| ---------------------------- | ---------------------------------------- |
| Prefix test data with `E2E_` | `"E2E_Course_{timestamp}"`               |
| Labels in double quotes      | `"Submit"`, `"Email"`                    |
| Paths without quotes         | `/academy/courses`                       |
| Be specific                  | `Click button "Save"` not `Click "Save"` |

---

## Proactive Behavior

- After code changes → suggest `/airis:create`
- Tests exist, not run → suggest `/airis:run`
- With Airflow: build → create test → run → done
