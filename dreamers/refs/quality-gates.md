# Quality Gates (MANDATORY)

Run a quality gate at every major handoff boundary. **If a gate fails, send back to the originating agent with specific issues — never route forward on a failed gate.**

---

## Gate 2 — Plan quality check

Before routing to Forge, read the plan file(s) and check every item:

- [ ] Plan file(s) named per naming convention (`plan-{n}[-{letter}]-{slug}.md`)
- [ ] Non-trivial features have an umbrella plan + sub-plans (not one monolithic plan)
- [ ] Each sub-plan has **Acceptance Criteria** — numbered, measurable, Forge-verifiable (not vague)
- [ ] Each sub-plan has **Test Cases for Probe** using Given/When/Then format for non-trivial cases
- [ ] Each sub-plan has a **Design Decisions** section using the structured format
- [ ] Each sub-plan has a **Rollback Boundary** declaration
- [ ] Each sub-plan references only files/paths that actually exist in the codebase — no invented paths
- [ ] Sub-plan splits are at natural seams, not arbitrary line-count cuts
- [ ] No sub-plan's testability depends on a sibling sub-plan that hasn't shipped yet
- [ ] Plan contains no code snippets (exception: interface/type contracts only)

**Any failure = spawn Nova as a subagent with the specific item(s) that failed.**

---

## Gate 3a — Pre-Sentinel: Implementation artifact exists

After Forge completes and before Sentinel:

- [ ] `forge/implementation.md` exists and is non-empty

If missing: send back to Forge.

---

## Gate 3b — Post-Forge-fixes: Implementation completeness

After Sentinel → Forge-fixes cycle completes and before Probe:

- [ ] Lists every file **changed** (with one-line reason per file)
- [ ] Lists every file **read for context**
- [ ] `How to test` section maps to the sub-plan's Test Cases for Probe
- [ ] Known limitations / follow-ups section is present (even if "none")

**Any missing field = send back to Forge.**

---

## Gate 4 — Post-Probe: Test coverage check

After Probe completes:

- [ ] AC coverage matrix exists — every AC from the plan is listed
- [ ] Every AC has at least one test mapped to it
- [ ] `probe/runbook.md` exists with exact commands and expected outputs
- [ ] If user-reported bug: `probe/regression-analysis.md` exists and answers all three questions

**Any missing item = send back to Probe.**

---

## Finding routing rule (non-negotiable)

Any finding in `findings.md` at any severity — critical, high, medium, or low — routes to Forge for a fix before Probe runs. No deferred or skipped findings. If Sentinel files it, Forge fixes it.

## Re-review rule

Only re-run Sentinel after Forge fixes if the original findings included critical or high severity. Medium/low fixes go directly to Probe — no Sentinel re-run required.
