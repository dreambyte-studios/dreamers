# Atlas Decisions

## 2026-03-18 — Bypassing Atlas for trivial changes: do not make agents self-sufficient

**Question:** Should CJ be able to invoke Forge (or other agents) directly, bypassing Atlas, for trivial code changes?

**Decision: No. Atlas remains the mandatory entry point. Fix friction through tighter routing shortcuts, not agent self-sufficiency.**

**Reasoning:**

1. **Forge's current blockers would halt direct invocation.** Forge reads `inbox.md` on startup and requires a plan file link. Without Atlas prep, both are missing and Forge stops. Bypassing Atlas does not save time — it causes Forge to write back to Atlas and wait anyway.

2. **Making agents self-sufficient creates two-mode agents.** An inbox-driven mode and a direct-invocation mode diverge in behavior. Safeguards (plan file check, PROJECT.md read, structured handoffs) erode in the direct mode. Over time the inconsistency causes mistakes.

3. **The friction is not "Atlas must run first" — it is in what Atlas does.** For a trivial fix, Atlas writes two sentences into Forge's inbox and names the shortcut route. That is the correct cost: low, consistent, and preserves the safeguard chain.

4. **The right fix is making the trivial shortcut explicit.** The existing routing shortcuts already cover this: `Atlas → Forge → Atlas → Echo → Atlas`. Atlas should invoke this immediately without ceremony for genuinely trivial changes.

**One genuine gap fixed:** Forge's rule "no plan file = stop" is too rigid. A one-line fix does not need a plan file. The rule should be "no plan file for non-trivial work = stop." Forge judges triviality by: single file, well-understood change, no cross-agent effects, and Atlas's inbox explicitly marks it trivial.

**What Atlas must do for trivial direct-to-Forge routing:**
- Confirm in chat the change qualifies as trivial
- Write a minimal inbox entry to Forge: context, what to change, done criteria
- State "no plan file needed — trivial change" explicitly in the inbox
- Invoke Forge immediately

**Applies to:** All agent invocation patterns. Atlas is always the entry point.

---

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

---

## 2026-03-18 — Atlas makes meta edits directly (skip Forge and Sentinel)

**Question:** For meta work (agent definitions, Kernel, config), can Atlas skip Forge and Sentinel entirely and make edits directly?

**Decision: Yes. Atlas edits meta files directly.**

**Reasoning:**

The prior decision (2026-03-18, "Meta-work routing shortcut") routed meta work as Atlas → Sentinel → Forge → Atlas. That was better than the full pipeline but still wrong. Here is why:

1. **Forge adds no value for meta work.** Forge's purpose is implementing non-trivial code where a dedicated implementation pass catches structural mistakes. Editing a markdown agent definition is not that. Routing through Forge just introduces a handoff with no upside.

2. **Sentinel pre-flight is not worthless but is disproportionate.** For straightforward prompt edits where the user has stated exactly what they want, a dedicated Sentinel pass is process theater. The real review mechanism for meta work is: CJ reads the diff before approving the commit. That is sufficient.

3. **Atlas already has the tools.** Atlas has Read, Write, Edit, Glob, Grep — the full toolkit needed for text file edits. There is no capability gap that Forge fills.

4. **The risk of Atlas making a bad edit is low and recoverable.** Agent definitions are version-controlled. A bad edit is a `git revert` away. The cost of a mistake is low; the cost of a multi-agent pipeline for every minor prompt tweak is high.

**Updated meta-work routing shortcut:**
```
Meta change: Atlas edits directly → user reviews diff → commit
```

**When this does NOT apply:**
- Redesigning the routing model or agent role boundaries (use Nova first)
- Any change with cross-agent downstream effects that are non-obvious (use Nova first)
- Changes where Atlas is uncertain what the correct edit is (ask CJ, not Sentinel)

**Supersedes:** The "Meta change: Atlas → Sentinel → Forge → Atlas" shortcut from the 2026-03-18 meta-work decision.

---

## 2026-03-18 — Atlas should NOT get the Agent tool for autonomous routing

**Question:** Should Atlas be given the Agent tool so it can invoke other agents autonomously without the user needing to invoke each one?

**Decision: No. Do not give Atlas the Agent tool.**

**Reasoning:**

1. **Loss of user visibility is the core problem.** The current model requires CJ to invoke each agent. That is not inefficiency — it is a checkpoint. CJ sees what each agent produces before the next one runs. Autonomous chaining removes those checkpoints. If Forge produces a bad implementation, Atlas-with-Agent-tool would hand it to Sentinel and Probe before CJ ever sees it.

2. **Failure propagation.** In a multi-agent chain, a bad output in step 2 corrupts every downstream step. With manual invocation, the blast radius is one agent. With autonomous routing, it is the whole pipeline.

3. **The Agent tool creates a different execution model.** Subagents spawned via the Agent tool run in isolated contexts. They do not share the parent's conversation history the same way. This changes how agents read their inbox, how they access plan files, and how handoff content works. It is a significant architectural change, not a convenience tweak.

4. **The current friction is acceptable and intentional.** Requiring CJ to invoke each agent keeps CJ in the loop on non-trivial work. The prior decision (2026-03-18, "Atlas autonomous routing") already removed unnecessary pausing for trivial routing decisions. What remains is deliberate human checkpointing.

5. **The right place to reduce friction is shortcut routing, not autonomous chaining.** If a task only needs Forge + Echo, Atlas should say that clearly and CJ invokes two agents instead of five. That preserves visibility without the risks of autonomous execution.

**What to do instead of the Agent tool:**
- Keep routing shortcuts tight and accurate so CJ invokes only the agents actually needed.
- Atlas states clearly in chat which agent is next and why, so each invocation is a low-friction decision for CJ.

**Applies to:** Any future proposal to add Agent tool to Atlas's tool list or to enable autonomous pipeline execution.
