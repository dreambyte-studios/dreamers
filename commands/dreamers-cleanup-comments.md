Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

Run a code comment cleanup pass across the codebase. Route: Atlas → Forge → Atlas → PR.

$ARGUMENTS

**Step 1 — Branch setup (Atlas)**
```
git checkout main && git pull origin main
git checkout -b chore/cleanup-comments
```

**Step 2 — Forge's task — comment cleanup rules (non-negotiable):**

Scan and edit source files according to these rules. No other changes — do not touch logic, formatting, or structure.

**Remove entirely:**
- Any comment referencing a plan file, requirement ID, or acceptance criterion (e.g. `// plan-14a R-7`, `// AC-9`, `// plan-13a req 2`, `// D15`, `// H-2`)
- Section divider lines (e.g. `// ---------------------------------------------------------------------------`, `// ===`, `// ---`)
- Comments that restate what the code obviously does (`// increment counter`, `// return true`)
- Commented-out code blocks
- Redundant JSDoc/KDoc that only repeats the function signature with no added meaning

**Rewrite to be concise:**
- Any comment longer than one line that could be said in one line without losing meaning
- Comments that explain *what* instead of *why* — rewrite to explain *why*, or delete if the why is obvious

**Keep as-is:**
- Comments that explain a non-obvious decision, constraint, or gotcha
- Public API documentation that callers need
- TODO/FIXME with a specific actionable note (not vague placeholders)
- License headers

**Step 3 — Single commit (Forge)**
Stage all changes and create one commit:
```
style: remove plan refs, dividers, and noise comments
```

**Step 4 — PR (Atlas)**
Push the branch and open a PR against main:
- Title: `style: remove plan refs, dividers, and noise comments`
- Body: brief summary of what was cleaned up

Report the PR URL.
