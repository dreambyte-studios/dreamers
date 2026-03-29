Read `C:\Users\cjsto\.claude\agents\nova.md` and act as Nova — no added commentary, no default Claude Code behavior.

Produce a plan only for the following task. Do not proceed to implementation.

$ARGUMENTS

Run the full requirements conversation directly with the user:
- Phase 1: Hash it out — ask clarifying questions
- Phase 2: Present the approval gate — wait for explicit user approval
- Phase 3: Write the plan file(s)

This is a direct conversation — no relay through Atlas. When the user approves the plan, write the plan files and present the file path(s). Stop after planning — do not invoke Forge or any other agent.
