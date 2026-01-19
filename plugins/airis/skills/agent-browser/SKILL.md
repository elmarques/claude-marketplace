---
name: agent-browser
description: Browser automation via visual AI. Use when executing ATML tests, interacting with web pages, or automating browser tasks. Core tool for Airis test execution.
---

# agent-browser

CLI for visual browser automation. Sees pages like a human — no selectors needed.

## Installation

```bash
npm install -g agent-browser
agent-browser install
```

## Core Workflow

```
open → snapshot -i → find @ref → interact → re-snapshot → verify
```

ALWAYS `snapshot -i` before interactions. Refs (`@N`) change each snapshot.

---

## Commands

### Browser Lifecycle

| Command    | Description         |
| ---------- | ------------------- |
| `open URL` | Open URL in browser |
| `close`    | Close browser       |
| `back`     | Navigate back       |
| `forward`  | Navigate forward    |
| `reload`   | Reload current page |

### Snapshots

| Command       | Description                |
| ------------- | -------------------------- |
| `snapshot`    | Page state                 |
| `snapshot -i` | With interactive refs (@N) |

Output: `@1 [button] "New Course"`, `@2 [input] "Email"`, etc.

### Interactions

| Command              | Description                    |
| -------------------- | ------------------------------ |
| `click @N`           | Click element at ref           |
| `fill @N "text"`     | Clear and fill input           |
| `type "text"`        | Type without clearing          |
| `press Enter`        | Press key (Enter, Tab, Escape) |
| `select @N "Option"` | Select dropdown option         |
| `check @N`           | Check checkbox                 |
| `uncheck @N`         | Uncheck checkbox               |
| `hover @N`           | Hover over element             |

### Waiting

| Command                | Description                 |
| ---------------------- | --------------------------- |
| `wait --text "text"`   | Wait for text to appear     |
| `wait --url "partial"` | Wait for URL to contain     |
| `wait --load`          | Wait for page load complete |
| `wait --time 1000`     | Wait milliseconds           |

### Screenshots

`screenshot path.png` — Airis: ALWAYS to `.airis/screenshots/`

### Getters

| Command        | Returns                 |
| -------------- | ----------------------- |
| `get url`      | Current page URL        |
| `get text @N`  | Text content of element |
| `get value @N` | Input value             |
| `get title`    | Page title              |

---

## Example: Login

```bash
open http://localhost:3000/login
snapshot -i                    # @1 "Email", @2 "Password", @3 "Sign in"
fill @1 "admin@example.com"
fill @2 "password123"
click @3
wait --url "/dashboard"
```

---

## ATML Translation Reference

| ATML Step                      | agent-browser Sequence                         |
| ------------------------------ | ---------------------------------------------- |
| `Navigate to /path`            | `open {baseUrl}/path`                          |
| `Click button "X"`             | `snapshot -i` → find @N with "X" → `click @N`  |
| `Click link "X"`               | `snapshot -i` → find @N with "X" → `click @N`  |
| `Fill "Label" with "value"`    | `snapshot -i` → find @N → `fill @N "value"`    |
| `Select "Option" from "Label"` | `snapshot -i` → find @N → `select @N "Option"` |
| `Press Enter`                  | `press Enter`                                  |
| `Wait for "text"`              | `wait --text "text"`                           |
| `Wait for URL contains "X"`    | `wait --url "X"`                               |

| ATML Assertion           | agent-browser Verification                |
| ------------------------ | ----------------------------------------- |
| `Toast contains "X"`     | `snapshot -i` → check "X" in output       |
| `Page shows "X"`         | `snapshot -i` → check "X" in output       |
| `Table has row "X"`      | `snapshot -i` → check row with "X" exists |
| `URL is /path`           | `get url` → compare                       |
| `Button "X" is disabled` | `snapshot -i` → check @N state            |

---

## Error Handling

Element not found → re-snapshot → retry → screenshot → fail

Timeout → increase wait, check action result

## Flags

| Flag       | Description         |
| ---------- | ------------------- |
| `--headed` | Show browser window |

## Tips

- `snapshot -i` before every interaction
- Re-snapshot after page changes
- `wait` for async operations
- Screenshot on failures
- `--headed` for debugging
