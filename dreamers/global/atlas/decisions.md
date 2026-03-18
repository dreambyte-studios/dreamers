# Atlas Decisions

## 2026-03-18 — Atlas autonomous routing (no user approval needed)

**Context:** CJ gave explicit feedback that Atlas should handle agent routing autonomously. The original Kernel required Atlas to tell the user which agent to invoke next and wait.

**Decision:** Atlas invokes the next agent directly without pausing to ask CJ. Only pause and ask CJ when:
1. A decision requires CJ's input (e.g., a URL, a choice between options)
2. CJ explicitly says "wait for my approval" or similar

**Applies to:** All agent routing in both global and repo-local workspaces.

**Kernel section to update:** "Routing model (Claude Code)" — step 5 currently says "Tell the user exactly which agent to invoke next." That must change to "Invoke the next agent directly."

---

## 2026-03-18 — Meta-work routing shortcut added

**Context:** The .claude repo tracks agent definitions, Dreamers knowledge, and config files. The full Nova→Forge→Sentinel→Probe→Echo pipeline is disproportionate for this type of work — no code is shipped, no tests can run, and the changed file is its own documentation.

**Decision:** Add a meta-work routing shortcut to the Kernel:

```
Meta change: Atlas → Sentinel → Forge → Atlas
```

- Sentinel runs **before** Forge for meta work (pre-flight review, not post-implementation review)
- Nova skipped: user intent is already clear; no design work needed
- Probe skipped: nothing to execute
- Echo skipped: the agent/config file itself is the artifact

**When shortcut does NOT apply:** Redesigning agent roles, restructuring the routing model, or changes with cross-agent downstream effects — those require Nova first.

**Applies to:** agent prompt edits (`agents/*.md`), Dreamers Kernel changes, config file edits (`CLAUDE.md`, `settings.json`), workspace convention updates (`PROJECT.md`).

---

## 2026-03-18 — Git root is C:\Users\cjsto\.claude
The user said "initialize in the current working directory (C:\Users\cjsto\.claude\dreamers)" but the files to track (agents/, CLAUDE.md, settings.json, settings.local.json) are siblings of dreamers/, not inside it. Therefore the git root must be `C:\Users\cjsto\.claude`, not the dreamers subdirectory.
