## Phase 1 — Planning

Read these refs before starting:
- `~/.claude/dreamers/refs/planning-protocol.md`
- `~/.claude/dreamers/refs/plan-rules.md`
- `~/.claude/dreamers/refs/feature-decomposition.md`
- `~/.claude/dreamers/refs/plan-content.md`
- `~/.claude/dreamers/refs/testing-mandate.md`
- `~/.claude/dreamers/refs/citation-accuracy.md`

Follow the Dreamers Kernel and output discipline from `CLAUDE.md`.

$ARGUMENTS

Run the full requirements conversation directly with the user:
- Phase 1: Hash it out — ask clarifying questions (one round)
- Phase 2: Present the approval gate — wait for explicit user approval
- Phase 3: Write the plan file(s)

Do not proceed to Phase 2 (implementation) until the user explicitly approves the plan.

---

## Phase 2 — Implementation

Read these refs before starting:
- `~/.claude/dreamers/refs/git-workflow.md`
- `~/.claude/dreamers/refs/quality-gates.md`
- `~/.claude/dreamers/refs/delegation.md`
- `~/.claude/dreamers/refs/close-out.md`
- `~/.claude/dreamers/refs/agent-recovery.md`

If the plan has sub-plans, also read:
- `~/.claude/dreamers/refs/sub-plan-loop.md`

The plan is already user-approved. Run Gate 2 (plan quality check) on the plan files, then orchestrate:

**Single plan route:** Forge → Sentinel → Probe → Close-out (Bolt handles push + PR)
**Sub-plan route:** Loop per sub-plan (see sub-plan-loop.md), then Close-out (Bolt handles push + PR)

Run quality gates at every handoff boundary per quality-gates.md. Follow delegation.md for all agent invocations (use Bolt for mechanical tasks like test runs, git push, PR creation, issue closing). Follow git-workflow.md for branching, commits, and push discipline. Follow close-out.md for retro and PR creation.

If the prompt references a GitHub issue number or URL, pass it to Bolt at close-out to close: `gh issue close <number> --comment "Resolved in <PR URL>"`.
