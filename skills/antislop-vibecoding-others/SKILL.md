---
name: antislop-vibecoding-others
description: "Anti-slop rules for everything else: AI acting as QA/tester, and .gitignore + sensitive-file separation. English mirror of docs/others.md."
allowed-tools: Read Write Edit Glob Grep Bash
---

# antislop-vibecoding-others

> Part of the antislop-vibecoding system. Load together with the core skill `antislop-vibecoding`.
> English mirror of the Indonesian source of truth `docs/others.md` (IDs match).

## Scope

Areas outside backend/frontend/UI-UX/database: testing/QA, security (testing), CI/CD, git & commits, DevOps, documentation.

## Rules

### OT-01 — AI acts as QA/tester

- **Tell:** a feature is declared done without testing; security isn't verified.
- **Why:** without testing, bugs and security holes ship to production. The user can't know if the site is safe.
- **Fix:** before declaring done, the AI acts as **QA/tester**:
  - Test **function** (happy path + edge cases), **limits** (rate limiting, empty/odd/hostile input), and **security** (SQL injection, unauthorized access, sensitive-data leaks).
  - Use the project's tools (test runner/framework); if none, test manually and record the steps.
  - **Report format:** a numbered list — (1) what was tested, (2) result (pass/fail), (3) findings + severity, (4) recommendations. See WF-08.

### OT-02 — .gitignore & sensitive files

- **Tell:** there's no `.gitignore` (or it's incomplete), and it's unclear which files are safe to push vs contain secrets.
- **Why:** otherwise credentials/sensitive data can be committed to GitHub — leaked and hard to undo.
- **Fix:** provide a **complete** `.gitignore`: deps (`node_modules/`), env (`.env*`), keys (`*.pem`, `*.key`, `*.p12`), `secrets/`, `credentials.json`, build (`dist/`, `build/`, `.next/`), logs, cache, OS/editor files. Then **document** what is pushed and what isn't, and **flag files that contain sensitive data**. Never commit secrets (keys/tokens/passwords/connection strings) — use env vars or a secret manager. (see WF-07)

## Checklist

All answers must be **yes**:

- [ ] Before finishing, function & security were tested as QA/tester and the results reported. (OT-01)
- [ ] `.gitignore` is complete; sensitive files are flagged and not pushed, and pushed vs not-pushed is documented. (OT-02)
