# Forge Inbox

## Task: Initialize git repo with selective .gitignore

**From:** Atlas
**Date:** 2026-03-18
**Routing:** Atlas → Forge → Atlas → Echo (trivial change, no Nova/Sentinel/Probe needed)

### Context
CJ wants to version-control the Claude Code global configuration directory. The git repo goes in `C:\Users\cjsto\.claude`. That directory already contains agents, dreamers, plugins, and config files.

### What was done
Atlas bootstrapped the global workspace (`C:\Users\cjsto\.claude\dreamers\global\`) and created `PROJECT.md`.

### What is needed
1. Run `git init` in `C:\Users\cjsto\.claude`
2. Create `C:\Users\cjsto\.claude\.gitignore` that:
   - Ignores everything by default (`*`)
   - Explicitly un-ignores (with `!`) the following:
     - `agents/` and all its contents
     - `dreamers/` and all its contents
     - `CLAUDE.md`
     - `settings.json`
     - `settings.local.json`
   - Ensures nested contents of tracked directories are also un-ignored (requires `!agents/**` etc.)
3. Stage the tracked files and make an initial commit with message: `chore: initial commit`

### Constraints
- Do NOT track `plugins/` or any other directory not listed above
- Do NOT modify any agent `.md` files in `agents/`
- The `.gitignore` itself should also be tracked (add it to the un-ignore list)
- `settings.local.json` may not exist yet — only add it if it exists; do not fail if absent

### Definition of Done
- `git status` in `C:\Users\cjsto\.claude` shows only tracked files staged/committed
- `.gitignore` is present and correct
- Initial commit exists

### Links
- PROJECT.md: `C:\Users\cjsto\.claude\dreamers\global\PROJECT.md`
- Atlas status: `C:\Users\cjsto\.claude\dreamers\global\atlas\status.md`
