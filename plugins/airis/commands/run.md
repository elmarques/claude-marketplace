---
description: Run ATML test file via agent-browser
argument-hint: <file> [--headed] [--test "name"]
---

# Run

## Rules

- **Coordinate only — never execute tests directly**
- One agent per test file — runs all tests in sequence
- Collect results + improvements, propose at end

## 1. Load

Read `.airis/CONFIG.md` → missing → suggest `/airis:setup`

Extract: baseUrl, headed, timeout, cleanup_prefix, @flows

Load credentials from `.env.local` → missing → stop, instruct user

Check baseUrl reachable → unreachable → AskUserQuestion start background dev server (infer from package.json)

## 2. Resolve Test File

Argument required → missing → stop: "Specify test file: `/airis:run {file.md}`"

Path: `.airis/tests/{argument}`

Parse file:

- Frontmatter: title, auth, headed
- Tests: each `# Heading`

`--test "name"` → filter to single test within file

## 3. Execute

1. Tell user: "Running: {file name}..."
2. Spawn: `Task(general-purpose)` with prompt:

```
Execute test file via agent-browser.

CONTEXT:
- baseUrl: {baseUrl}
- headed: {headed flag or config}
- Auth flow: {auth flow steps from CONFIG if needed}
- @flows: {flows from CONFIG}

TEST FILE:
{full test file content}

PROCESS:
1. Auth flow first (if specified in frontmatter)
2. For each test (# Heading):
   - Setup — expand @flows, execute
   - Steps — translate ATML → agent-browser, execute sequentially
   - Verify — translate assertions, check each
   - Note any improvements observed
3. Continue to next test (unless --fail-fast)

TRANSLATION (ATML → agent-browser):
- Navigate to /path → open {baseUrl}/path
- Click button "X" → snapshot -i → find @N → click @N
- Fill "Label" with "value" → snapshot -i → find @N → fill @N "value"
- Wait for "text" → wait --text "text"
- Assertions → snapshot -i → check output

CONSTRAINTS:
- Always snapshot -i before interactions
- On fail → screenshot .airis/screenshots/{test-slug}-{step}.png

RETURN FORMAT:
RESULTS:
- {test name}: PASS
- {test name}: FAIL — {step}: {reason}

IMPROVEMENTS: Only if you hit actual issues (timing, wrong selector, flaky). Clean run → "none"
- {test name}: {suggestion}
- none
```

3. Parse response → collect results + improvements

## 4. Report

```
[PASS] Create course
[FAIL] Edit course → Step 3: element not found
Results: 3/4 passed
```

## 5. Cleanup

Delete test data (`cleanup_prefix`, default `E2E_`) via UI. Infer from:

- What tests created
- CONFIG.md app context

Skip: `--no-cleanup`

## 6. Improve

If improvements collected:

1. Consolidate and present:

   ```
   Improvements found:
   - Create course: Add wait before step 3
   - Edit course: Use more specific button label
   ```

2. AskUserQuestion: Apply improvements? | Skip

3. If yes → apply changes to test file, show diff

## Flags

| Flag           | Effect                       |
| -------------- | ---------------------------- |
| `--headed`     | Show browser window          |
| `--test "X"`   | Run only test matching `# X` |
| `--no-cleanup` | Skip cleanup phase           |
| `--fail-fast`  | Stop on first failure        |

## Notes

- One agent per file — fresh context, runs all tests in sequence
- Screenshots always to `.airis/screenshots/`
- Improvements come from executor's real experience running the tests
