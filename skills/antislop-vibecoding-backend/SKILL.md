---
name: antislop-vibecoding-backend
description: "Anti-slop rules for backend work: rate limiting per user, and SQL injection / XSS / CSRF protection. English mirror of docs/backend.md."
allowed-tools: Read Write Edit Glob Grep Bash
---

# antislop-vibecoding-backend

> Part of the antislop-vibecoding system. Load together with the core skill `antislop-vibecoding`.
> English mirror of the Indonesian source of truth `docs/backend.md` (IDs match).

## Scope

API, services, authentication, input validation, security (rate limiting, injection protection), error handling, logging.

## Rules

### BE-01 — Rate limit per user

- **Tell:** there is no per-user request limit; endpoints can be called without limit.
- **Why:** without a limit, bots or abuse can flood the server (downtime or runaway cost) — the site can be taken down. **No number means no protection.**
- **Fix:** a concrete limit number is **mandatory** — never leave it blank or write "X". Start from these defaults, then tune per project:
  - General per user: **60 req/min**
  - Per IP (not logged in): **100 req/min**
  - Auth endpoints (login / OTP / password reset): **5–10 attempts/min** per IP
  - Heavy endpoints (upload / generate / export): **10 req/min** per user
  - When exceeded: return **HTTP 429** (+ `Retry-After` header)
  - Also cap request body size and add timeouts.
  - **Where to record:** store the numbers in config/env (e.g. `RATE_LIMIT_PER_USER=60`) and note them in project docs — don't hardcode them scattered around.
  - **Precedence:** check the **per-user** limit first; if not logged in, then **per-IP**.

### BE-02 — Prevent SQL injection & sanitize input

- **Tell:** queries are built by concatenating user input (e.g. `"SELECT * FROM pelanggan WHERE nama = '" + input + "'"`), or user input isn't validated.
- **Why:** SQL injection and other intrusions can read, change, or delete data — the site can be breached.
- **Fix:** protect against **SQL injection and other intrusions**:
  - Always use **parameterized queries / prepared statements**; never concatenate strings.
  - Validate & sanitize all input to the expected type/format.
  - **XSS:** never render raw user input as HTML; escape/sanitize output.
  - **CSRF:** add token protection to data-changing requests (POST/PUT/DELETE).
  - Never put raw user data into queries, shell commands, or HTML output without protection.
  - Apply least-privilege database access.

## Checklist

All answers must be **yes**:

- [ ] Request limits per user/IP are set (concrete, not "X") and rate limiting is applied. (BE-01)
- [ ] All queries use parameterized queries; user input is validated & sanitized. (BE-02)
