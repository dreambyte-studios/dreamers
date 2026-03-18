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

---

## 2026-03-18 — Git commit pending: forge.md trivial-work plan-gate softened

**For:** CJ (manual) or main Claude instance (if Bash available)

**What changed:** `agents/forge.md` role responsibilities — replaced hard "no plan = stop" rule with a tiered requirement: trivial work (single-file edits, small fixes) proceeds without a plan when Atlas marks the item `trivial` or when the change is clearly self-contained; non-trivial work still requires a plan file.

**Commit command:**
```
cd /c/Users/cjsto/.claude
git add agents/forge.md
git commit -m "chore(agents): soften forge plan-gate — trivial work proceeds without plan

Trivial work (single-file edits, small fixes) no longer hard-stops if no
plan file is present, provided Atlas marked the inbox item as trivial or
the change is clearly self-contained. Non-trivial work (new features,
refactors, multi-file changes) still requires an explicit plan file link."
```
