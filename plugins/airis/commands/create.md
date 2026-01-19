---
description: Create new ATML test file with wizard
argument-hint: [test-name]
---

# Create

Wizard → well-formed ATML test file.

## 1. Load

Read `.airis/CONFIG.md` → missing → suggest `/airis:setup`

Extract: `@flows`, `baseUrl`

## 2. Explore Codebase

Routes, components, UI patterns, existing tests → inform suggestions and translation.

## 3. Gather Info

**Name:** argument | AskUserQuestion: "Test name?"

**Path:** suggest based on name, AskUserQuestion: Use suggested | Custom

**Auth:** AskUserQuestion: list `@login-*` from CONFIG | None

**Tests:** AskUserQuestion: "What flows to test?" → parse into test names

## 4. Per Test

AskUserQuestion: "Steps?" → "Verify?"

## 5. Generate File

Write to `.airis/tests/{path}`:

```yaml
---
title: {Name}
auth: {auth-choice}
---

# {Test 1 name}

## Steps

1. {translated step}
2. {translated step}

## Verify

- {translated assertion}
- {translated assertion}

---

# {Test 2 name}

## Steps
...
```

## Translation

Natural language → ATML. Infer from context. Reference: `skills/airis/SKILL.md`

**Style:** concise, imperative. `Click button "Save"` not `You should click the Save button`.

## 6. Confirm

Show file. AskUserQuestion: Create (recommended) | Edit | Abort

## Output

```
Created: .airis/tests/{path}
Run: /airis:run {path} --headed
```

## Tips

- User vague → clarify, suggest patterns, reference existing tests
- One goal per test, 3-7 steps, 2-4 assertions
