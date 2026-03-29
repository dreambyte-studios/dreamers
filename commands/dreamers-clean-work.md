Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

Run a between-milestone maintenance pass. No implementation, no planning, no agents — Atlas does all of this directly.

$ARGUMENTS

**Step 1 — Improvements audit**
Read `.dreamers/atlas/improvements.md` (repo-local) AND `~/.claude/dreamers/global/atlas/improvements.md` (global). For each open item:
- Decide: action now, defer with a reason, or close as no longer relevant.
- If actionable as a direct text edit to an agent file or config (meta work shortcut applies): make the edit now.
- If it requires Forge or a full pipeline: defer it — add a note with why it needs a pipeline and what skill to use.
- Remove actioned/closed items from the file. Leave only open deferred items with their defer reasons.

**Step 2 — Plan file cleanup**
In `.dreamers/plans/` (repo-local) and `~/.claude/dreamers/global/plans/` (global), for each `plan-*.md` file:
- Check if its associated PR is merged (`gh pr list --state merged` or `gh pr view <number>`).
- **Merged:** delete the plan file. The PR description is the lasting record.
- **Open or not yet created:** leave it.
- Report what was deleted and what was kept (with reason).

**Step 3 — Workspace file reset**
For every agent workspace in `.dreamers/` (repo-local), wipe all live status files back to "No active work / No pending items":
- `atlas/status.md`, `forge/status.md`, `nova/status.md`, `probe/status.md`, `sentinel/status.md`

Prune any workspace file (across all agents) that exceeds ~200 lines or ~20KB — delete stale content, rewrite to only current actionable items.

**Step 4 — Memory contradiction scan**
Read all files in `~/.claude/projects/[repo]/memory/` AND `~/.claude/dreamers/global/`. Check for:
- Tech stack drift
- Architecture pivots not propagated
- Milestone status that has fallen behind
- Rule conflicts across agent definitions

**Propose** all memory changes to the user — do not auto-apply. Present a list and wait for approval.

**Step 5 — Report**
Summarise in chat:
- Improvements actioned / deferred / closed (one line each)
- Plan files deleted / kept
- Workspace files pruned
- Proposed memory updates (if any) — list and await user approval before applying
