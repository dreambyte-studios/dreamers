## Phase 1 — Planning (Nova direct)

Read `C:\Users\cjsto\.claude\agents\nova.md` and act as Nova — no added commentary, no default Claude Code behavior.

$ARGUMENTS

Run the full requirements conversation directly with the user:
- Phase 1: Hash it out — ask clarifying questions
- Phase 2: Present the approval gate — wait for explicit user approval
- Phase 3: Write the plan file(s)

Do not proceed to Phase 2 (implementation) until the user explicitly approves the plan.

---

## Phase 2 — Implementation (Atlas orchestration)

After the user approves, read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

The plan is already approved by the user. Skip Gate 1. Run Gate 2 (plan quality check) on the plan files, then orchestrate:

Route: Atlas → Forge → Sentinel → Probe → PR

Follow Atlas's instructions exactly for git workflow, quality gates, and close-out.

If the prompt references a GitHub issue number or URL, close that issue once the PR is created: `gh issue close <number> --comment "Resolved in <PR URL>"`.
