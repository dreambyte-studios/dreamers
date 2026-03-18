# Sentinel Outbox

## [ROUTED - 2026-03-18] Handoff to Forge — Plan 1: Meta Routing Shortcut

Cycle complete. Archived to `archive/2026/03/review-20260318-plan1.md`.

---

## [ROUTED - 2026-03-18] Handoff to Forge — Plan 2: Enforce Sentinel Outputs and Inbox Archiving

**Date:** 2026-03-18
**Verdict:** APPROVED WITH FIXES — implement redlined text

### Summary
Both improvements are approved with two Should Fix wording refinements applied. No Must Fix items. Forge should implement the approved text verbatim into the two target files.

### What Forge must implement

#### Change 1: sentinel.md
Insert the following as a new `###` subsection immediately after the "Review outputs" bullet block (after the line `- \`findings.md\`: itemized issues with severity...`, before `## Handoffs`):

```markdown
### Output file creation (mandatory)
Before writing any review output, ensure these files exist in the active sentinel workspace for this cycle (repo-local `./.dreamers/sentinel/` or global `~/.claude/dreamers/global/sentinel/` — match where the inbox item originated); create them if absent:
- `findings.md`
- `review.md`
- `outbox.md`

Sentinel's DoD is not met if any of these three files is missing after review completes.
```

**Target file:** `C:\Users\cjsto\.claude\agents\sentinel.md`
**Insertion point:** After line 59 (the `- \`findings.md\`: itemized issues...` line), before the blank line before `## Handoffs`.

#### Change 2: atlas.md
Insert the following as a new `###` subsection at the end of the "Pruning + archiving policy" section (after the `Never delete history.` line, before `## Routing model`):

```markdown
### Inbox archiving
Each time Atlas reads an agent's `inbox.md` during routing: if the file contains more than ~10 items total, prune all `[ROUTED]` and `[COMPLETE]` items into `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md` within that agent's workspace before proceeding. Update that agent's `archive/index.md`.
```

**Target file:** `C:\Users\cjsto\.claude\agents\atlas.md`
**Insertion point:** After the `Never delete history.` line, before `## Routing model`.

### Constraints
- Use approved text verbatim.
- Touch no other lines in either file.
- Verify heading hierarchy is intact after each insertion.
- One commit per file change, or one combined commit — both are acceptable. Use conventional commits referencing plan-2.

### Definition of Done
- `sentinel.md` contains the Output file creation subsection at the correct location.
- `atlas.md` contains the Inbox archiving subsection at the correct location.
- No other lines altered in either file.
- Forge writes `implementation.md` and posts outbox handoff to Atlas.

### References
- Plan: `C:\Users\cjsto\.claude\dreamers\global\plans\plan-2-enforce-sentinel-outputs-and-inbox-archiving.md`
- Review: `C:\Users\cjsto\.claude\dreamers\global\sentinel\review.md`
- Findings: `C:\Users\cjsto\.claude\dreamers\global\sentinel\findings.md`
