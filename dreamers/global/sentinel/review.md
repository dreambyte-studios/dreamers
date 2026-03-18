# Sentinel Review — Plan 2: Enforce Sentinel Outputs and Inbox Archiving

**Date:** 2026-03-18
**Verdict:** APPROVED WITH FIXES — two Should Fix wording refinements; no Must Fix items

---

## Summary

Both proposed changes are correct in intent and structure. No design issues. Two wording refinements improve precision without changing scope.

---

## Improvement 1: sentinel.md

**Verdict:** Approve with Should Fix (Finding 1.1)

### Approved text (with redline applied)

```markdown
### Output file creation (mandatory)
Before writing any review output, ensure these files exist in the active sentinel workspace for this cycle (repo-local `./.dreamers/sentinel/` or global `~/.claude/dreamers/global/sentinel/` — match where the inbox item originated); create them if absent:
- `findings.md`
- `review.md`
- `outbox.md`

Sentinel's DoD is not met if any of these three files is missing after review completes.
```

### Placement
Insert as a new `###` subsection immediately after the "Review outputs" bullet block (after line 59 in the current file, before the blank line that precedes `## Handoffs`).

---

## Improvement 3: atlas.md

**Verdict:** Approve with Should Fix (Finding 3.1)

### Approved text (with redline applied)

```markdown
### Inbox archiving
Each time Atlas reads an agent's `inbox.md` during routing: if the file contains more than ~10 items total, prune all `[ROUTED]` and `[COMPLETE]` items into `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md` within that agent's workspace before proceeding. Update that agent's `archive/index.md`.
```

### Placement
Insert as a new `###` subsection at the end of the "Pruning + archiving policy" section (after the "Never delete history." line, before `## Routing model`).

---

## What Forge must do

1. In `sentinel.md`: insert the approved "Output file creation (mandatory)" subsection after the existing "Review outputs" bullets, before `## Handoffs`.
2. In `atlas.md`: insert the approved "Inbox archiving" subsection at the end of the "Pruning + archiving policy" section, before `## Routing model`.
3. Use approved text verbatim.
4. Touch no other lines in either file.
5. Verify heading hierarchy is intact after insertion.
6. Commit with conventional commits referencing plan-2.

## What Forge must NOT do

- Do not paraphrase approved text.
- Do not touch any section outside the specified insertion points.
