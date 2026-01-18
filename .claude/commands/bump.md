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
3. If no `$2` → auto-detect:
   - Read commits since last tag (or all if no tags)
   - `BREAKING CHANGE:` or `!:` in message → major
   - `feat:` → minor
   - Otherwise → patch
4. Calculate new version
5. Update files:
   - `plugins/$1/.claude-plugin/plugin.json` → `version`
   - `.claude-plugin/marketplace.json` → find plugin by name, update `version`
6. Show diff summary

## Output

```
{plugin}: {old} → {new} ({type})

Updated:
- plugins/{plugin}/.claude-plugin/plugin.json
- .claude-plugin/marketplace.json
```

## Constraints

- Do NOT commit — user decides when
- Do NOT modify package.json
- Fail if plugin not found
