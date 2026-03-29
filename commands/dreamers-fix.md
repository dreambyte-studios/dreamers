Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

Run the Tier 1 bug fix pipeline for the following bug:

$ARGUMENTS

Route: Atlas → Forge → Probe → Atlas (skip Nova and Sentinel)

Follow Atlas's instructions exactly.

Only use this route if all four Tier 1 conditions are met (shipped feature, obvious cause, clearly scoped fix, no new logic/files/data model changes). If any condition fails, escalate to the full Tier 2 pipeline automatically:

**Tier 2 escalation — two-phase approach:**
1. **Planning:** Read `nova.md` and act as Nova. Run the full requirements conversation directly with the user. Wait for explicit approval.
2. **Implementation:** Read `atlas.md` and act as Atlas. The plan is already approved — skip Gate 1. Run Gate 2, then: Forge → Sentinel → Probe → PR.

If the change touches `mobile/` runtime files, distribute a Firebase preview build before opening the PR per the project CLAUDE.md Distribution section.

If the prompt references a GitHub issue number or URL, close that issue once the PR is created: `gh issue close <number> --comment "Resolved in <PR URL>"`.
