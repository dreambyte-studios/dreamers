# Atlas Improvements

## 2026-03-18 — Plan 1 cycle (meta routing shortcut)

1. ~~**sentinel.md** — Add note that Sentinel must create `findings.md`, `review.md`, and `outbox.md` if absent; these are required by every Definition of Done.~~ **CLOSED — plan-2**

2. **atlas.md (delegation protocol)** — Add a rule: when CJ provides an explicit decision in chat, Atlas may apply it in the current Forge pass without a separate Sentinel review; must record it in `decisions.md` and the Forge implementation log. **OPEN**

3. ~~**Kernel or atlas.md** — Add inbox archiving guidance: items marked `[ROUTED]` or `[COMPLETE]` should be archived to `archive/YYYY/MM/` when inbox exceeds ~10 items or ~100 lines.~~ **CLOSED — plan-2**

Retro: `C:\Users\cjsto\.claude\dreamers\global\atlas\retros\retro-20260318.md`

---

## 2026-03-18 — Plan 2 cycle (enforce sentinel outputs + inbox archiving)

1. **atlas.md or Kernel — Forge commit proxy rule:** When Atlas runs the full pipeline without a separate Forge Bash invocation, add a protocol for Atlas to surface the commit message as a pending manual step. Currently commits silently fail.

2. **atlas.md — cycle-close checklist:** Add a short checklist Atlas runs at every cycle close: (a) mark all agent outboxes [ROUTED], (b) verify Forge committed, (c) write retro, (d) update improvements.md. Prevents [PENDING] leaks.

Retro: `C:\Users\cjsto\.claude\dreamers\global\atlas\retros\retro-20260318-plan2.md`

---

## 2026-03-18 — Sentinel split decision

1. ~~**sentinel.md — Add `### Review lenses` subsection:** Internal checklist approach.~~ **SUPERSEDED** — full parallel sub-reviewer architecture adopted instead. See decisions.md "Sentinel parallel sub-reviewers via Agent tool: ADOPTED".

## 2026-03-18 — Sentinel sub-reviewer adoption

1. **sentinel.md** — Updated: tool list now includes `agent`; sub-reviewer protocol, three lenses, consolidation procedure, and sub-file archiving rule added. **CLOSED — applied directly**

Decision: `C:\Users\cjsto\.claude\dreamers\global\atlas\decisions.md` — "Sentinel parallel sub-reviewers via Agent tool: ADOPTED"
