# Forge Inbox

## [ROUTED - 2026-03-18 complete] Implement: Add Meta Routing Shortcut to atlas.md

**From:** Atlas (routed from Sentinel)
**Date:** 2026-03-18
**Plan:** `C:\Users\cjsto\.claude\dreamers\global\plans\plan-1-meta-routing-shortcut-agent-updates.md`
**Project context:** `C:\Users\cjsto\.claude\dreamers\global\PROJECT.md`

### Context
Sentinel reviewed the proposed meta routing shortcut and approved it with two redlines (exception condition sharpened, path reference rooted). The approved text is below. Forge must insert it verbatim into `atlas.md`.

### What was done
- Atlas identified gap in routing shortcuts, authored proposed text, prepared plan
- Sentinel reviewed, found 2 wording issues, produced redlined approved text
- Review: `C:\Users\cjsto\.claude\dreamers\global\sentinel\review.md`
- Findings: `C:\Users\cjsto\.claude\dreamers\global\sentinel\findings.md`

### What is needed
Insert the following text into `C:\Users\cjsto\.claude\agents\atlas.md` after line 93 (the "Fix a review finding" bullet), before the line that reads "Atlas decides which shortcut applies. When in doubt, run the full pipeline.":

```
- Meta work (agent/config updates): Atlas → Sentinel → Forge → Atlas (skip Nova, Probe, Echo)
  - Applies when: updating agent definitions (agents/), dreamers workspace files (~/.claude/dreamers/ or ./.dreamers/), or config files (CLAUDE.md, settings.json, settings.local.json)
  - Skip rationale: Nova — scope is clear from the proposed change; Probe — nothing to test; Echo — no external docs affected
  - Exception: if the change affects more than one agent's behavior or alters a cross-agent protocol (routing model, delegation protocol, retro process), add Nova before Sentinel
```

### Constraints
- Use the approved text verbatim — do not paraphrase or reformat
- Touch only the Routing shortcuts section (lines 89–95 area)
- Verify bullet list indentation is consistent with existing entries after insertion
- Do not alter any other line in atlas.md

### Definition of Done
- New shortcut entry present in atlas.md verbatim
- No other lines altered
- Forge writes `forge/implementation.md` summarizing what was changed
- Forge posts outbox handoff to Atlas

### References
- Plan: `C:\Users\cjsto\.claude\dreamers\global\plans\plan-1-meta-routing-shortcut-agent-updates.md`
- Sentinel review: `C:\Users\cjsto\.claude\dreamers\global\sentinel\review.md`

---

## Implement: Plan 2 — Enforce Sentinel Outputs and Inbox Archiving

**From:** Atlas (routed from Sentinel)
**Date:** 2026-03-18
**Plan:** `C:\Users\cjsto\.claude\dreamers\global\plans\plan-2-enforce-sentinel-outputs-and-inbox-archiving.md`
**Project context:** `C:\Users\cjsto\.claude\dreamers\global\PROJECT.md`

### Context
CJ approved two improvements. Sentinel reviewed and approved both with Should Fix wording refinements. No Must Fix items. Forge implements the redlined text verbatim.

### What was done
- Atlas created plan-2
- Sentinel reviewed, applied two wording refinements (workspace qualifier, routing-moment trigger scope)
- Review: `C:\Users\cjsto\.claude\dreamers\global\sentinel\review.md`
- Findings: `C:\Users\cjsto\.claude\dreamers\global\sentinel\findings.md`

### Change 1: sentinel.md

**Target:** `C:\Users\cjsto\.claude\agents\sentinel.md`

Insert the following as a new `###` subsection immediately after the "Review outputs" bullet block (after the line `- \`findings.md\`: itemized issues with severity (critical/high/medium/low) + suggested remediation (targeted, not rewrites)`, before `## Handoffs`):

```markdown
### Output file creation (mandatory)
Before writing any review output, ensure these files exist in the active sentinel workspace for this cycle (repo-local `./.dreamers/sentinel/` or global `~/.claude/dreamers/global/sentinel/` — match where the inbox item originated); create them if absent:
- `findings.md`
- `review.md`
- `outbox.md`

Sentinel's DoD is not met if any of these three files is missing after review completes.
```

### Change 2: atlas.md

**Target:** `C:\Users\cjsto\.claude\agents\atlas.md`

Insert the following as a new `###` subsection at the end of the "Pruning + archiving policy" section (after the `Never delete history.` line, before `## Routing model`):

```markdown
### Inbox archiving
Each time Atlas reads an agent's `inbox.md` during routing: if the file contains more than ~10 items total, prune all `[ROUTED]` and `[COMPLETE]` items into `archive/YYYY/MM/inbox-YYYYMMDD-HHMM.md` within that agent's workspace before proceeding. Update that agent's `archive/index.md`.
```

### Constraints
- Use approved text verbatim — do not paraphrase or reformat
- Touch no other lines in either file
- Verify heading hierarchy is intact after each insertion
- Commit with conventional commits referencing plan-2
- Write `implementation.md` and post outbox handoff to Atlas

### Definition of Done
- `sentinel.md` contains the Output file creation subsection at the correct location
- `atlas.md` contains the Inbox archiving subsection at the correct location
- No other lines altered in either file
- `forge/implementation.md` written
- Outbox handoff to Atlas posted

---

## Task: Initialize git repo with selective .gitignore

**From:** Atlas
**Date:** 2026-03-18
**Routing:** Atlas → Forge → Atlas → Echo (trivial change, no Nova/Sentinel/Probe needed)

### Context
CJ wants to version-control the Claude Code global configuration directory. The git repo goes in `C:\Users\cjsto\.claude`. That directory already contains agents, dreamers, plugins, and config files.

### What was done
Atlas bootstrapped the global workspace (`C:\Users\cjsto\.claude\dreamers\global\`) and created `PROJECT.md`.

### What is needed
1. Run `git init` in `C:\Users\cjsto\.claude`
2. Create `C:\Users\cjsto\.claude\.gitignore` that:
   - Ignores everything by default (`*`)
   - Explicitly un-ignores (with `!`) the following:
     - `agents/` and all its contents
     - `dreamers/` and all its contents
     - `CLAUDE.md`
     - `settings.json`
     - `settings.local.json`
   - Ensures nested contents of tracked directories are also un-ignored (requires `!agents/**` etc.)
3. Stage the tracked files and make an initial commit with message: `chore: initial commit`

### Constraints
- Do NOT track `plugins/` or any other directory not listed above
- Do NOT modify any agent `.md` files in `agents/`
- The `.gitignore` itself should also be tracked (add it to the un-ignore list)
- `settings.local.json` may not exist yet — only add it if it exists; do not fail if absent

### Definition of Done
- `git status` in `C:\Users\cjsto\.claude` shows only tracked files staged/committed
- `.gitignore` is present and correct
- Initial commit exists

### Links
- PROJECT.md: `C:\Users\cjsto\.claude\dreamers\global\PROJECT.md`
- Atlas status: `C:\Users\cjsto\.claude\dreamers\global\atlas\status.md`
