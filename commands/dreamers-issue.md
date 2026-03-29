Create a GitHub issue for the following request:

$ARGUMENTS

**Step 1 — Repo detection**
Run `gh repo view --json nameWithOwner` to confirm the current repo. If not in a git repo or no remote is set, ask the user which repo to target before proceeding.

**Step 2 — Label check**
Run `gh label list` to see available labels. Pick the most appropriate existing label(s) — do not invent labels that don't exist. Common mappings:
- New capability → `feature`
- Something broken → `bug`
- Docs gap → `documentation`
- Unclear ownership → `question`

**Step 3 — Draft the issue**
Write a focused, surface-level issue. The goal is to capture enough context for a planner to understand scope — not to solve the problem.

Use the template at `~/.claude/dreamers/global/templates/github-issue.md` as the structure. Keep the body concise — no padding, no restatement of the title. Nova will plan the details.

**Step 4 — Create**
Run `gh issue create` with the drafted title, body, and label(s). Report the issue URL when done.
