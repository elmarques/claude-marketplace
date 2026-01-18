---
description: Bump plugin version (semver)
argument-hint: <plugin> [patch|minor|major]
---

# Bump Plugin Version

## Arguments

- `$1` — plugin name (required)
- `$2` — version type (optional) → `patch` | `minor` | `major`

## Process

1. Validate plugin exists at `plugins/$1/.claude-plugin/plugin.json`
2. Read current version from plugin.json
3. Find last tag matching `$1-v*`
4. If no `$2` → auto-detect:
   - List commits since last tag (or all if no tags)
   - `BREAKING CHANGE:` or `!:` in message → major
   - `feat:` → minor
   - Otherwise → patch
5. Calculate new version
6. Update files:
   - `plugins/$1/.claude-plugin/plugin.json` → `version`
   - `.claude-plugin/marketplace.json` → find plugin by name, update `version`
7. Commit: `chore($1): bump to {new}`
8. Create tag: `$1-v{new}`

## Output

```
{plugin}: {old} → {new} ({type})

Updated:
- plugins/{plugin}/.claude-plugin/plugin.json
- .claude-plugin/marketplace.json

Committed: chore({plugin}): bump to {new}
Tagged: {plugin}-v{new}
```

## Constraints

- Do NOT push — user decides when
- Do NOT modify package.json
- Fail if plugin not found
