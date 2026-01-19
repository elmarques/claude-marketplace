---
description: Show Airis usage guide
model: haiku
---

# Help

Print the following:

```
Airis — AI-native test runner

SETUP
  /airis:setup      Initialize .airis/ in project
  /airis:help       Show this help

WORKFLOW
  /airis:create     Wizard to create test file
  /airis:run <file> Execute test file

TYPICAL FLOW
  1. /airis:setup           (first time only)
  2. Edit .airis/CONFIG.md  (baseUrl, auth flows)
  3. Add credentials to .airis/.env.local
  4. /airis:create
  5. /airis:run my-test.md --headed

FLAGS (/airis:run)
  --headed       Show browser
  --test "X"     Run single test
  --no-cleanup   Keep test data
  --fail-fast    Stop on first failure

FILES
  .airis/CONFIG.md      Settings, auth flows, app context
  .airis/.env.local     Credentials (gitignored)
  .airis/tests/         ATML test files
  .airis/screenshots/   Failure captures (gitignored)

TROUBLESHOOTING
  agent-browser not found → npm install -g agent-browser && agent-browser install
  Credentials missing     → add to .airis/.env.local
  Connection refused      → start dev server (baseUrl must be reachable)
  Test fails              → try --headed, check screenshots

MORE INFO
  https://github.com/elmarques/claude-marketplace/tree/main/plugins/airis
```
