Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

Add comprehensive, production-grade logging to the project. Work through the phases in order. Do not touch code until the user approves the audit findings.

$ARGUMENTS

---

## Phase 1 — Atlas audit

Before invoking any agent, Atlas performs this audit directly.

**Stack detection:**
- Read `CLAUDE.md` (project-level), `package.json`, `build.gradle`, `pyproject.toml`, `go.mod` — whatever exists
- Identify: language(s), runtime, existing dependencies, package manager

**Existing logging inventory:**
- Search for `console.log`, `console.error`, `console.warn`, `print(`, `println(`, `Log.d(`, `Log.e(`, `logger.`, `logging.` and any existing logging framework imports
- Note: how many, where, what kind (debug noise vs. genuine signal)

**Key instrumentation areas:**
- Identify: app entry point / startup, server/service init, route handlers or API endpoints, database access layer, external API/HTTP calls, auth flows, background jobs or workers, top-level error handlers

**Framework recommendation:**
Based on the detected stack, recommend one framework. Common mappings:
- Node.js / TypeScript → **pino** (structured, fast, production-ready) or **winston** (more configurable)
- Python → **structlog** (structured) or **loguru** (simple, powerful)
- Kotlin / Android → **Timber**
- Go → **zap** or **slog** (stdlib)
- Other → surface to user and ask

If the stack is ambiguous or no clear framework fits, stop and ask the user before proceeding.

**Surface to user:**
Present a concise summary:
1. Detected stack and package manager
2. Existing logging found (count, locations, quality assessment)
3. Recommended framework + one-line rationale + rejected alternative(s)
4. List of areas that will be instrumented
5. Ask explicitly: **"Does this look right? Approve or tell me what to adjust before I proceed."**

Do not proceed to Phase 2 until the user explicitly approves.

---

## Phase 2 — Branch setup (Atlas)

```
git checkout main && git pull origin main
git checkout -b chore/add-logging
```

---

## Phase 3 — Forge implementation

Invoke Forge with the full audit context. Forge's task:

**Install the approved framework:**
- Use the project's package manager (detected in Phase 1)
- Install the framework and any required transports/plugins

**Set up logger configuration:**
Create a central logger module (e.g. `src/lib/logger.ts`, `src/utils/logger.py`):
- **Dev environment**: pretty-printed, human-readable console output with colours and timestamps
- **Prod environment**: structured JSON to stdout (so log aggregators like Datadog, Logtail, or CloudWatch can consume it)
- Environment detected via `NODE_ENV`, `APP_ENV`, or equivalent for the stack
- Log levels: ERROR, WARN, INFO, DEBUG — default to INFO in prod, DEBUG in dev

**Logging standards:** Follow `~/.claude/dreamers/global/templates/logging-standards.md` exactly. Forge must read this file before writing any log calls.

**Replace existing noise:**
- Remove or replace all raw `console.log`, `console.error`, `print()`, `println()` etc. with the appropriate log level call
- Assess each: if it was genuine signal, keep it at the right level; if it was debug noise, either downgrade to DEBUG or remove

**Add logging to instrumentation areas** identified in Phase 1.

Single commit when complete:
```
chore: add structured logging with [framework name]
```

Record files changed in `forge/implementation.md`.

---

## Phase 4 — Sentinel review

Invoke Sentinel. Focus areas to call out explicitly in the prompt:
- Any log call that could expose PII, credentials, or sensitive data
- Log calls inside tight loops or hot paths that would generate noise at INFO level
- Missing stack traces on ERROR-level catches
- Inconsistent log levels (e.g. INFO used for what should be DEBUG)
- Any `console.log` / raw print that Forge missed

Re-review rule: only re-run Sentinel if findings include critical or high severity.

---

## Phase 5 — PR

Push the branch and open a PR against main:
- Title: `chore: add structured logging with [framework name]`
- Body: framework chosen, environments configured, areas instrumented, existing noise removed

Report the PR URL to the user.
