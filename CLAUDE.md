# Claude Marketplace

Personal plugin marketplace for Claude Code.

## Structure

```
.claude-plugin/marketplace.json   → plugin catalog
plugins/{name}/                   → individual plugins
  .claude-plugin/plugin.json      → plugin manifest
  commands/                       → slash commands
  CLAUDE.md                       → plugin dev guidelines
```

## Versioning

Each plugin has `version` in two places — keep in sync:

- `plugins/{name}/.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json` → `plugins[name].version`

Use `/bump {plugin} [patch|minor|major]` to update.

## Writing Style

Concise markdown. Every token counts.

- Drop subjects/articles — `Read file` not `You should read the file`
- Symbols — `→` (result), `|` (or), `CAPS:` (attention)
- Imperatives, lists > prose
- Unresolved questions → end of section
