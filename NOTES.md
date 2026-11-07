# Notes

## CLAUDE.md

I kept the commands, including how to run one test file or one test by name, plus the rules Claude could get wrong without being told: CommonJS only, `node:test` rather than Jest or Mocha, the `{ "error": "<message>" }` format, and data access only through `db/store.js`. The architecture section covers what the code doesn't make obvious, such as why the `listen` guard must stay, that store data persists between tests in a file, and that nothing loads `.env`.

I left out a file-by-file listing and the Express routes, because Claude can read those from the code and they would go stale. I also left out the course instructions, since they are one-off and already in the README, and anything sensitive, because this file is committed.

## Permission rules

- **Allow:** `npm test`, `npm run lint`, and `node --test`. These are safe and run constantly, so prompting for them only slows things down. `npm run dev` isn't included because it starts a long-running server.
- **Ask:** `git push`, because pushing is visible to others and I want to confirm it each time.
- **Deny:** reading `.env`, and force-pushing with `--force` or `-f`.

Each command rule is written for both Bash and PowerShell, because Claude Code can use either on Windows and a Bash rule doesn't cover the PowerShell tool.

Without the deny rules, Claude could read real secrets from `.env` into a session, or a force-push could overwrite shared history on the remote.

These rules have limits. `Read(./.env)` only blocks the Read tool, not a shell command like `cat .env`. The force-push rules match the flag only straight after `git push`, so `git push origin main --force` isn't denied, though the ask rule still prompts for it.

## Verification

- [ ] `claude --version`:
- [ ] `/memory` shows `CLAUDE.md` loaded:
- [ ] `/permissions` shows the allow, ask, and deny rules:
- [ ] "How do I run the tests here?" is answered from `CLAUDE.md`:
