# Plan 2 — Enforce Sentinel Outputs and Inbox Archiving

## Status
In progress

## Problem
Two gaps in the Dreamers agent system:

1. **Sentinel output enforcement**: `sentinel.md` lists `review.md`, `findings.md`, and `outbox.md` as required files in the workspace section, but never explicitly states that Sentinel *must create them if they don't exist* as part of completing a review. The DoD check in inbox handoffs refers to these files, but Sentinel's own prompt has no explicit creation mandate. This is a silent failure mode.

2. **Inbox bloat**: As cycles accumulate, any agent's `inbox.md` will fill with `[ROUTED]` and `[COMPLETE]` items. There is no guidance on when or how to prune these. An inbox growing beyond ~10 items becomes noise that obscures actionable work.

## Scope
- Edit `C:\Users\cjsto\.claude\agents\sentinel.md` — add explicit mandate that Sentinel creates `findings.md`, `review.md`, and `outbox.md` if they do not exist before writing review output.
- Edit `C:\Users\cjsto\.claude\agents\atlas.md` — add inbox archiving guidance: prune `[ROUTED]` and `[COMPLETE]` items from any agent's inbox to `archive/YYYY/MM/` when the inbox exceeds ~10 items.

## Non-goals
- No changes to any other agent files
- No changes to routing logic
- No changes to review checklist or findings format

## Proposed Changes

### sentinel.md change
In the "Review outputs" section (or as a new "Output file creation" subsection under "Sentinel role responsibilities"), add:

> Before writing any review output, ensure the following files exist in the sentinel workspace; create them if absent:
> - `findings.md`
> - `review.md`
> - `outbox.md`
>
> Sentinel's DoD is not met if any of these three files is missing after review completes.

### atlas.md change
In the "Pruning + archiving policy" section (or as a new subsection under "Required directories & files"), add inbox archiving guidance:

> **Inbox archiving**: When any agent's `inbox.md` exceeds ~10 items, prune all `[ROUTED]` and `[COMPLETE]` items to `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md`. Update the agent's `archive/index.md`. Leave only unrouted / in-progress items in the active `inbox.md`.

## Acceptance Criteria
1. `sentinel.md` contains an explicit instruction that Sentinel must create `findings.md`, `review.md`, and `outbox.md` if absent before writing review output.
2. `sentinel.md` explicitly states DoD is not met without all three files.
3. `atlas.md` contains inbox archiving guidance with the ~10 item threshold and the archive destination pattern.
4. No other sections in either file are altered.
5. Changes are committed with conventional commits referencing this plan.

## Risks
- Low risk: text-only changes to agent prompts, no behavioral side-effects outside the Dreamers system.

## References
- Improvement 1 and Improvement 3 from retro cycle: approved by CJ 2026-03-18
- `C:\Users\cjsto\.claude\dreamers\global\atlas\retros\retro-20260318.md`
