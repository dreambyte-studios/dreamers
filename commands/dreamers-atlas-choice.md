Evaluate the following task and choose the correct pipeline:

$ARGUMENTS

## How to choose

**Use the Tier 1 route if ALL four conditions are true:**
1. The feature it belongs to is fully shipped (PR merged)
2. The bug is directly and obviously caused by the just-shipped feature
3. The fix is clearly scoped — describable in one sentence
4. No new logic, no new files, no data model changes — purely corrective

**Use the full pipeline for everything else:** new features, non-trivial bugs, anything with ambiguity, any data model or API change, any fix that doesn't meet all four Tier 1 conditions.

---

## Tier 1 — Simple fix

Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

Route: Atlas → Forge → Probe → Atlas (skip Nova and Sentinel)

## Tier 2 — Full pipeline

### Phase 1 — Planning (Nova direct)
Read `C:\Users\cjsto\.claude\agents\nova.md` and act as Nova — no added commentary, no default Claude Code behavior. Run the full requirements conversation directly with the user. Do not proceed until the user explicitly approves.

### Phase 2 — Implementation (Atlas orchestration)
After user approval, read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas. The plan is already approved — skip Gate 1. Run Gate 2, then orchestrate: Forge → Sentinel → Probe → PR.

---

## After choosing, state your choice and reasoning in one sentence, then proceed immediately.

## Rules that apply to both routes

- If the change touches `mobile/` runtime files, distribute a Firebase preview build before opening the PR per the project CLAUDE.md Distribution section.
- If the prompt references a GitHub issue number or URL, close that issue once the PR is created: `gh issue close <number> --comment "Resolved in <PR URL>"`.
- Follow Atlas's git workflow: branch from main, single push immediately before `gh pr create`.
