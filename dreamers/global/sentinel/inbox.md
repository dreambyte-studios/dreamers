# Sentinel Inbox

## [ROUTED - 2026-03-18] Review: Add Meta Routing Shortcut to atlas.md

**From:** Atlas
**Date:** 2026-03-18
**Plan:** `C:\Users\cjsto\.claude\dreamers\global\plans\plan-1-meta-routing-shortcut-agent-updates.md`
**Project context:** `C:\Users\cjsto\.claude\dreamers\global\PROJECT.md`

### Context
The Dreamers routing shortcuts section in `atlas.md` has no entry for meta work — updates to agent definitions, dreamers knowledge base files, or config files. This gap forces Atlas to decide ad hoc each time. Adding a named shortcut makes routing deterministic.

This change is being applied using the meta flow itself (Atlas → Sentinel → Forge → Atlas) before the shortcut formally exists, which is intentional: Sentinel reviews the proposed wording before Forge writes it.

### What was done (by Atlas)
- Identified the gap in routing shortcuts
- Authored the proposed wording for the new shortcut (see below)
- Created plan-1 at the path above
- Prepared this inbox handoff

### What is needed from Sentinel
Review the proposed shortcut text below and the target file for:

1. **Correctness** — Does the shortcut accurately describe when to apply it? Is the skip rationale sound?
2. **Ambiguity** — Is any condition (the exception rule in particular) vague enough to cause inconsistent application?
3. **Conflicts** — Does the new entry overlap with or contradict any existing shortcut?
4. **Placement** — Should it be appended at the end of the shortcuts list, or does ordering matter?
5. **Wording tightness** — Is the exception condition ("tweak vs redesign") clear enough, or does it need a sharper criterion?

### Proposed shortcut text (to be inserted into atlas.md)
```
- Meta work (agent/config updates): Atlas → Sentinel → Forge → Atlas (skip Nova, Probe, Echo)
  - Applies when: updating agent definitions (agents/), dreamers knowledge base (dreamers/), or config files (CLAUDE.md, settings.json, settings.local.json)
  - Skip rationale: Nova — scope is clear from the proposed change; Probe — nothing to test; Echo — no external docs affected
  - Exception: if scope is ambiguous (tweak vs redesign), add Nova before Sentinel
```

### Target file
`C:\Users\cjsto\.claude\agents\atlas.md` — specifically the "Routing shortcuts (for simple tasks)" section (lines 89–95).

### Constraints
- Do not alter any other section of `atlas.md`
- Do not implement the change — that is Forge's job
- Produce a clear approve/reject/revise verdict with specific redline suggestions if revising

### Definition of Done
Sentinel has written findings to `sentinel/findings.md` and a verdict (approve / revise with redlines / reject with rationale) to `sentinel/review.md`, then posted a handoff to `sentinel/outbox.md` addressed to Atlas.

---

## Review: Plan 2 — Enforce Sentinel Outputs and Inbox Archiving

**From:** Atlas
**Date:** 2026-03-18
**Plan:** `C:\Users\cjsto\.claude\dreamers\global\plans\plan-2-enforce-sentinel-outputs-and-inbox-archiving.md`
**Project context:** `C:\Users\cjsto\.claude\dreamers\global\PROJECT.md`

### Context
CJ approved two improvements from the retro cycle:

1. **Improvement 1 (sentinel.md)**: Add explicit requirement that Sentinel must create `findings.md`, `review.md`, and `outbox.md` if they don't exist. These are required outputs — Sentinel's DoD is not met without them.
2. **Improvement 3 (atlas.md)**: Add inbox archiving guidance — prune `[ROUTED]` and `[COMPLETE]` items from any agent's inbox to `archive/YYYY/MM/` when the inbox grows beyond ~10 items.

Both are text-only changes to agent prompt files. The meta flow applies (Atlas → Sentinel → Forge → Atlas).

### What was done (by Atlas)
- Created plan-2 at the path above covering both improvements
- Drafted proposed change text (in plan-2)
- Prepared this inbox handoff

### What is needed from Sentinel
Review both proposed changes against:

1. **Correctness** — Does the sentinel.md addition accurately mandate file creation at the right point in the workflow?
2. **Placement** — Where in each file should each addition land to be most actionable? (Near the "Review outputs" section for sentinel.md; near "Pruning + archiving policy" for atlas.md.)
3. **Completeness** — Is the DoD statement for sentinel.md strong enough? Does it cover `outbox.md` or only `findings.md`/`review.md`?
4. **Inbox archiving threshold** — Is ~10 items the right trigger, or should it be tied to a different condition?
5. **Conflicts** — Does inbox archiving guidance in atlas.md conflict with any existing pruning/archiving rule?

### Proposed text for sentinel.md
Location: after the "Review outputs" bullet block (currently lines 58–59), as a new subsection under "Sentinel role responsibilities":

```
### Output file creation (mandatory)
Before writing any review output, ensure these files exist in the sentinel workspace; create them if absent:
- `findings.md`
- `review.md`
- `outbox.md`

Sentinel's DoD is not met if any of these three files is missing after review completes.
```

### Proposed text for atlas.md
Location: as a new subsection within "Required directories & files", or appended to the "Pruning + archiving policy" section:

```
### Inbox archiving
When any agent's `inbox.md` exceeds ~10 items, Atlas must prune all `[ROUTED]` and `[COMPLETE]` items into `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md` within that agent's workspace. Update `archive/index.md`. Leave only unrouted / in-progress items in the active `inbox.md`.
```

### Target files
- `C:\Users\cjsto\.claude\agents\sentinel.md`
- `C:\Users\cjsto\.claude\agents\atlas.md`

### Constraints
- Do not alter any other sections in either file
- Do not implement — that is Forge's job
- Produce approve/revise/reject verdict with specific redline text if revising

### Definition of Done
Sentinel has written itemized findings to `sentinel/findings.md`, a verdict to `sentinel/review.md`, and a handoff to `sentinel/outbox.md` addressed to Atlas.
