Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

A plan already exists. Skip Nova and go straight to implementation for the following task:

$ARGUMENTS

Route: Atlas → Forge → Sentinel → Probe → Echo → PR

The prompt must include a path to the existing plan file. If no plan file path is provided, stop and ask for it before proceeding — do not invent or skip the plan.

If the prompt references a GitHub issue number or URL, close that issue once the PR is created: `gh issue close <number> --comment "Resolved in <PR URL>"`.
