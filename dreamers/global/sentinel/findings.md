# Sentinel Findings — Plan 2: Enforce Sentinel Outputs and Inbox Archiving

**Date:** 2026-03-18
**Plan:** `C:\Users\cjsto\.claude\dreamers\global\plans\plan-2-enforce-sentinel-outputs-and-inbox-archiving.md`
**Targets:** `C:\Users\cjsto\.claude\agents\sentinel.md`, `C:\Users\cjsto\.claude\agents\atlas.md`

---

## Improvement 1: sentinel.md — Output file creation mandate

### Finding 1.1 — Workspace qualifier missing (SHOULD FIX)

**Criterion:** Wording tightness
**Severity:** Low

Proposed text says "ensure these files exist in the sentinel workspace" without specifying which workspace (repo-local `./.dreamers/sentinel/` vs global `~/.claude/dreamers/global/sentinel/`). A Sentinel agent new to a repo will not know which to create files in.

**Redline:**
```
Before writing any review output, ensure these files exist in the sentinel workspace; create them if absent:
```
Replace with:
```
Before writing any review output, ensure these files exist in the active sentinel workspace for this cycle (repo-local `./.dreamers/sentinel/` or global `~/.claude/dreamers/global/sentinel/` — match where the inbox item originated); create them if absent:
```

### Finding 1.2 — Heading placement is correct (PASS)

**Criterion:** Placement
The new `### Output file creation (mandatory)` subsection placed after the existing `### Review outputs` bullets is correct. Heading hierarchy is consistent with `### Plan alignment checks` and `### Review checklist` at lines 44 and 49.

### Finding 1.3 — DoD statement is strong (PASS)

**Criterion:** Completeness
"Sentinel's DoD is not met if any of these three files is missing after review completes." — unambiguous, correct scope.

---

## Improvement 3: atlas.md — Inbox archiving guidance

### Finding 3.1 — Trigger condition needs specificity (SHOULD FIX)

**Criterion:** Correctness
**Severity:** Low-Medium

"When any agent's inbox.md exceeds ~10 items, Atlas must prune..." implies a standing obligation Atlas watches proactively. Atlas only reads inboxes during routing steps. The trigger should be scoped to that moment.

**Redline:**
```
When any agent's `inbox.md` exceeds ~10 items, Atlas must prune all `[ROUTED]` and `[COMPLETE]` items into `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md` within that agent's workspace.
```
Replace with:
```
Each time Atlas reads an agent's `inbox.md` during routing: if the file contains more than ~10 items total, prune all `[ROUTED]` and `[COMPLETE]` items into `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md` within that agent's workspace before proceeding.
```

### Finding 3.2 — Archive destination is correct (PASS)

**Criterion:** Correctness
"Within that agent's workspace" correctly places the archive file under the receiving agent's `archive/` directory, not Atlas's. No conflict with existing pruning rules.

### Finding 3.3 — Threshold is appropriate (PASS)

**Criterion:** Threshold calibration
~10 items matches the approximate-threshold pattern already used (lines ~200 / ~20KB). Consistent.

### Finding 3.4 — No conflicts with existing pruning rules (PASS)

**Criterion:** Conflicts
Existing pruning policy covers active notes exceeding ~200 lines/~20KB. Inbox archiving is additive, not contradictory.

---

## Summary

| # | Finding | Severity | Status |
|---|---------|----------|--------|
| 1.1 | Workspace qualifier missing in sentinel.md mandate | Low | SHOULD FIX |
| 1.2 | Heading placement correct | — | PASS |
| 1.3 | DoD statement strong | — | PASS |
| 3.1 | Trigger condition too passive — scope to routing moment | Low-Med | SHOULD FIX |
| 3.2 | Archive destination correct | — | PASS |
| 3.3 | Threshold appropriate | — | PASS |
| 3.4 | No conflicts | — | PASS |

No Must Fix items. Both Should Fix items are wording-only refinements. Proceed to implement with redlines applied.
