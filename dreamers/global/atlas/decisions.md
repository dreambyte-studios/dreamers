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

## 2026-03-18 — Sentinel: unified agent vs. specialized sub-reviewers

**Question:** Should Sentinel be split into parallel specialized sub-reviewers (e.g. correctness, security, maintainability) following the pattern Claude Code uses for code review?

**Decision: No. Keep Sentinel unified. Improve it with an internal lens checklist, not a split.**

**Reasoning:**

**1. The parallelism argument does not apply here.**
Claude Code's multi-reviewer pattern works because subagents run concurrently via the Agent tool. The prior decision (2026-03-18) explicitly rejected giving Atlas the Agent tool. Without parallelism, splitting Sentinel into three agents means three serial CJ invocations, three inbox handoffs, and three Atlas routing steps — for output that one agent already produces.

**2. The artifacts Sentinel reviews do not justify specialization.**
Sentinel currently reviews markdown files: agent prompts, plan documents, config. A dedicated "security reviewer" applied to a markdown prompt produces a near-empty report in most cycles. Specialization creates overhead proportional to the artifact count, not the risk. The risk ceiling for this type of work is low.

**3. Lens separation already exists inside the current format.**
The review.md format (Must Fix / Should Fix / Nice to Have / Questions / Risk Notes) separates by action priority — which is more operationally useful than separating by lens. If Sentinel misses a lens today, the fix is adding a lens checklist to the prompt, not spawning additional agents.

**4. Pipeline complexity is already the main friction point.**
Both completed cycles have produced improvements about handoff overhead, commit proxying, and cycle-close gaps. Adding 2-3 Sentinel sub-agents makes the friction worse, not better.

**5. When splitting would make sense (and the conditions are not yet met):**
Specialized reviewers become worth the cost when: (a) Atlas has the Agent tool and can run them concurrently, AND (b) the codebase being reviewed contains real code where security, performance, and correctness genuinely require different expertise per-file. Neither condition holds for Dreamers today.

**What to do instead — add an internal lens checklist to sentinel.md:**
Sentinel should explicitly work through lenses in order before writing findings:
- Correctness: does implementation match acceptance criteria?
- Security: are there trust boundaries, injection risks, or privilege violations?
- Maintainability: is the change legible, consistent with conventions, and not introducing hidden coupling?
- Operational risk: what breaks if this is wrong?

This produces the same coverage as four specialized agents at zero additional cost.

**Proposed sentinel.md update:** Add a `### Review lenses` subsection immediately before `### Review outputs` that lists the four lenses with one-line definitions. Sentinel works through each lens before writing findings. No other changes needed.

**Applies to:** Any future proposal to split Sentinel or add specialized review agents.

---

## 2026-03-18 — Sentinel parallel sub-reviewers via Agent tool: ADOPTED

**Original question:** Should Sentinel get the Agent tool and spin up three parallel sub-reviewers (correctness, security, maintainability), consolidate their outputs, and hand a unified result to CJ — with the human checkpoint unchanged?

**Original decision (2026-03-18):** Architecture sound. Deferred. Gate condition: applied to a real multi-file codebase.

**Revised decision (2026-03-18): Implement now.**

**Why the gate condition is met:**
CJ has clarified that Dreamers is designed for production code as its primary use case. The markdown-only workload was temporary setup work, not the steady state. The gate condition — "applied to a repo with real code across multiple files" — describes exactly the intended operating context. The only remaining objection is invalidated. The architecture was already deemed sound. Implement now.

**Why the prior rejection does not apply:**
The earlier parallel-sub-reviewer rejection (2026-03-18, "Sentinel unified agent vs. specialized sub-reviewers") addressed Atlas chaining the full pipeline autonomously. This is different — Agent tool is scoped to a single Sentinel step. CJ still reviews Sentinel's consolidated output before Atlas routes forward. Blast radius for sub-reviewer failure is identical to Sentinel producing weak output today.

**Why it is mechanically valid:**
Subagents spawned via Agent can read files and write outputs to disk. Sentinel consolidates. The chain works. Critical caveat: subagents have no access to the parent's conversation history. Sentinel must inject all context explicitly in each sub-task prompt — plan file path, implementation paths, PROJECT.md path, lens definition, output file path, severity scale. Thin prompts produce thin output. The consolidation ceiling is prompt quality.

**Three review lenses for production code:**

1. **Correctness** — Does the implementation satisfy every acceptance criterion? Logic errors, off-by-ones, missing edge cases, requirement divergence, incorrect caller contract assumptions. Output: `sentinel/sub-correctness.md`

2. **Security** — Trust boundary violations, injection risks, privilege escalation, unsafe defaults, secrets in code, unvalidated input paths, missing authorization checks. For agent prompts: instructions that bypass human checkpoints or exfiltrate context. Output: `sentinel/sub-security.md`

3. **Maintainability** — Legibility, convention consistency, hidden coupling, dead code, conflicting conventions, naming quality, structural debt introduced by this change. Output: `sentinel/sub-maintainability.md`

Note: Operational risk is a cross-cutting concern, not an independent lens. It is covered by correctness (what breaks if logic is wrong) and security (unsafe defaults, missing guardrails). A fourth sub-reviewer adds overlap, not coverage.

**Updated tool list for sentinel.md:**
Add `Agent` to frontmatter tool list alongside read, search, edit. Final list: `[read, search, edit, agent]`.

**How Sentinel instructs sub-reviewers (required sub-task prompt content):**

Each sub-task prompt must be self-contained. Sentinel injects all of the following — no assumptions about inherited context:

```
You are a focused code reviewer. You have one lens only: [LENS NAME].

Lens definition: [one-sentence definition from the three lenses above]

Files to review:
- Plan: [absolute path to plan file]
- Implementation: [absolute paths to changed files]
- Project context: [absolute path to PROJECT.md]

Severity scale: critical / high / medium / low
- critical: blocks merge; introduces data loss, security breach, or broken core functionality
- high: must fix before merge; significant correctness or security gap
- medium: should fix; maintainability or minor correctness issue
- low: nice to have; style, naming, minor coupling

Your task:
1. Read every file listed above.
2. Review only through your assigned lens. Do not comment on issues outside your lens.
3. Write your findings to: [absolute path to sub-*.md output file]

Output format for [output file]:
# Sub-review: [LENS NAME]
## [severity] — [short title]
**Location:** [file:line or section]
**Issue:** [what is wrong]
**Remediation:** [specific fix, not a rewrite]

Write one entry per issue. If you find nothing, write: `# Sub-review: [LENS NAME]\nNo issues found.`
Do not write to any other file. Do not output findings in chat.
```

**How sub-reviewers output so Sentinel can consolidate:**

Each sub-reviewer writes exactly one file:
- `sentinel/sub-correctness.md`
- `sentinel/sub-security.md`
- `sentinel/sub-maintainability.md`

Sentinel consolidation procedure:
1. Read all three sub-*.md files.
2. Deduplicate: same issue flagged by multiple lenses = one entry at the highest severity assigned by any lens.
3. Write `findings.md` — all deduplicated issues with severity, location, remediation.
4. Write `review.md` — Summary, Must Fix (critical/high), Should Fix (medium), Nice to Have (low), Questions, Risk Notes.
5. Archive sub-files: move to `archive/YYYY/MM/sub-review-YYYYMMDD-HHMM/` (one folder per cycle, three files inside). Update `archive/index.md`.
6. Write outbox.md handoff to Atlas.

If any sub-reviewer output is missing or malformed, Sentinel notes it in review.md under Risk Notes and proceeds with available input. It does not silently drop the gap.

**Applies to:** sentinel.md tool list, sentinel.md review procedure, any future proposal to modify Sentinel's review architecture.

---

## 2026-03-18 — Atlas is NOT an order-taker (Critical Behavioral Directive)

**Directive from CJ:** Atlas must think critically about every request before acting. Blind execution is a failure mode, not a feature.

**Decision:** Atlas operates under the following non-negotiable behavioral rules:

1. **Evaluate before executing.** Every request gets assessed for soundness — approach, risks, hidden assumptions — before Atlas does anything. "CJ asked for it" is not sufficient justification to proceed.

2. **Push back when the idea has flaws.** If a request has a weak approach, a better alternative exists, or the risks outweigh the benefits, Atlas raises this in chat before acting. It proposes a counter rather than silently complying.

3. **Ask rather than assume.** When a request is ambiguous or the right path is unclear, Atlas asks CJ a focused question. It does not pick the most convenient interpretation and proceed.

4. **Sound + bulletproof = proceed.** Atlas only executes when it has independently concluded the idea is sound and the approach is defensible. If not, it says so.

**What this is not:** This is not a license to litigate every request. For clear, low-risk, well-scoped work, critical evaluation takes seconds and proceeds. The pushback behavior activates when there is a genuine flaw, a significant risk, or a clearly better path.

**Applies to:** All Atlas behavior, all workspaces, always.

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
