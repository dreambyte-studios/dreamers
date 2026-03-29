Run a between-milestone maintenance pass. No implementation, no planning, no agents — do all of this directly.

Follow the Dreamers Kernel and output discipline from `CLAUDE.md`.

$ARGUMENTS

**Step 1 — Improvements audit**
Read `.dreamers/atlas/improvements.md` (repo-local) AND `~/.claude/dreamers/global/atlas/improvements.md` (global). For each open item:
- Decide: action now, defer with a reason, or close as no longer relevant.
- If actionable as a direct text edit to an agent file or ref (meta work): make the edit now.
- If it requires Forge or a full pipeline: defer it — add a note with why and which skill to use.
- Remove actioned/closed items. Leave only open deferred items with defer reasons.

**Step 2 — Plan file cleanup**
In `.dreamers/plans/` (repo-local) and `~/.claude/dreamers/global/plans/` (global), for each `plan-*.md`:
- Check if its associated PR is merged (`gh pr list --state merged` or `gh pr view <number>`).
- **Merged:** delete the plan file. The PR description is the lasting record.
- **Open or not yet created:** leave it.
- Report what was deleted and what was kept (with reason).

**Step 3 — Workspace file reset (Bolt)**
Invoke **Bolt** to wipe all live status files in `.dreamers/` (repo-local) back to "No active work / No pending items":
- `forge/status.md`, `probe/status.md`, `sentinel/status.md`
- Use `printf 'No active work.\n' > <path>` for each file.

After Bolt completes, prune any workspace file (across all agents) that exceeds ~200 lines or ~20KB — delete stale content, rewrite to only current actionable items (this requires judgment — do it directly).

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
- Proposed memory updates (if any)
