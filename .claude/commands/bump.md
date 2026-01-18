---
description: Bump plugin version (semver)
argument-hint: <plugin> [patch|minor|major]
---

# Bump

## Arguments

- `$1` — plugin name
- `$2` — version type (optional) → `patch` | `minor` | `major`

## Process

### 1. Validate

- No `$1` → list available plugins at `plugins/`, abort
- Plugin not found at `plugins/$1/.claude-plugin/plugin.json` → abort
- Working directory dirty → abort with `git status`

### 2. Gather

- Read current version from `plugins/$1/.claude-plugin/plugin.json`
- Find last tag matching `$1-v*`

### 3. Detect Type

If `$2` provided → use it.

Otherwise, analyze commits since last tag **filtered by path** `plugins/$1/`:

| Pattern in message         | Type  |
| -------------------------- | ----- |
| `BREAKING CHANGE:` or `!:` | major |
| `feat:`                    | minor |
| otherwise                  | patch |

No tag exists → require explicit `$2`, abort if missing.

### 4. Preview

Show and confirm via AskUserQuestion:

```
{plugin}: {old} → {new} ({type})

Commits analyzed:
- {short_sha} {message}
- ...

Will update:
- plugins/{plugin}/.claude-plugin/plugin.json
- .claude-plugin/marketplace.json

Will commit: chore({plugin}): bump to {new}
Will tag: {plugin}-v{new}
```

Options: Confirm | Abort

### 5. Execute

1. Update version in both files
2. `git add` both files
3. `git commit -m "chore($1): bump to {new}"`
4. `git tag $1-v{new}`

### 6. Output

```
Done.

{plugin}: {old} → {new}
Tag: {plugin}-v{new}

Next: git push origin main --tags
```

## Constraints

- Do NOT push
- Do NOT modify package.json
- Abort on any validation failure
