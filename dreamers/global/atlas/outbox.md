# Atlas Outbox

## 2026-03-18 — Git commit pending: atlas.md meta-work routing shortcut update

**For:** CJ (manual) or main Claude instance (if Bash available)

**What changed:** `agents/atlas.md` routing shortcuts section — replaced `Atlas → Sentinel → Forge → Atlas` meta-work shortcut with `Atlas edits directly` pattern.

**Commit command:**
```
cd /c/Users/cjsto/.claude
git add agents/atlas.md
git commit -m "chore(agents): simplify meta-work routing — Atlas edits directly

Replace Atlas → Sentinel → Forge → Atlas shortcut with Atlas edits directly.
Atlas has the full tool set for text file edits; Forge and Sentinel add no
value for straightforward agent/config updates. User diff review before commit
is the review gate. Nova carve-out preserved for non-obvious cross-agent changes.

Decision: decisions.md 2026-03-18 (Atlas makes meta edits directly)"
```

**Note:** If the plan-2 commit (sentinel.md + atlas.md inbox archiving) has not yet been made, consolidate both into a single `git add` before committing:
```
git add agents/sentinel.md agents/atlas.md dreamers/
```

Then push: `git push -u origin master`
