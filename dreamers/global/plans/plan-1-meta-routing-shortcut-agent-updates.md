# Plan 1 — Add Meta Routing Shortcut for Agent/Config Updates

## Status
COMPLETE — 2026-03-18

## Goal
Add a named routing shortcut to the "Routing shortcuts" section of `C:\Users\cjsto\.claude\agents\atlas.md` that covers meta work: updates to agent definitions, the dreamers knowledge base, or config files.

## Motivation
Currently the routing shortcuts section has no entry for meta work (edits to `agents/`, `dreamers/`, `CLAUDE.md`, `settings.json`, `settings.local.json`). Without an explicit shortcut, Atlas must decide ad hoc. A named shortcut makes the decision deterministic and reviewable.

## Proposed Change

**Target file:** `C:\Users\cjsto\.claude\agents\atlas.md`

**Section:** "Routing shortcuts (for simple tasks)"

**New entry to append:**
```
- Meta work (agent/config updates): Atlas → Sentinel → Forge → Atlas (skip Nova, Probe, Echo)
  - Applies when: updating agent definitions (agents/), dreamers knowledge base (dreamers/), or config files (CLAUDE.md, settings.json, settings.local.json)
  - Skip rationale: Nova — scope is clear from the proposed change; Probe — nothing to test; Echo — no external docs affected
  - Exception: if scope is ambiguous (tweak vs redesign), add Nova before Sentinel
```

## Scope
- One file modified: `C:\Users\cjsto\.claude\agents\atlas.md`
- No code, no tests, no external docs

## Pipeline
Atlas → Sentinel (review) → Forge (implement) → Atlas

## Definition of Done
- New shortcut entry is present in the "Routing shortcuts" section of `atlas.md`
- Wording is unambiguous: when to use it, what to skip, the exception condition
- No other sections of `atlas.md` are altered
