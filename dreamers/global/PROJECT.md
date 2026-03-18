# PROJECT.md — Dreamers Global Workspace

## Purpose
This is the global (cross-repo / evergreen) knowledge base for the Dreamers agent system.

## Tech Stack
- No application code — this is a configuration and knowledge workspace
- Shell: bash (Unix syntax on Windows via Claude Code)
- Version control: git (being initialized at `C:\Users\cjsto\.claude`)

## Repo Structure
```
C:\Users\cjsto\.claude\
  agents/               # Agent definition markdown files (tracked)
  dreamers/             # Agent knowledge base (tracked)
    global/             # Global workspace root
      atlas/            # Atlas orchestrator state
      plans/            # Plan files
      PROJECT.md        # This file
  CLAUDE.md             # Global Claude instructions (tracked)
  settings.json         # Claude Code settings (tracked)
  settings.local.json   # Local settings override (tracked)
  plugins/              # NOT tracked (gitignored)
```

## Conventions
- All plans: `C:\Users\cjsto\.claude\dreamers\global\plans\plan-{n}-{slug}.md`
- Commit style: conventional commits preferred
- No direct modification of agent `.md` files in `agents/` without explicit user request

## Constraints
- Never delete history; archive stale content
- `plugins/` and all other directories not in the tracked list must remain gitignored

## Key Files
- `C:\Users\cjsto\.claude\CLAUDE.md` — global Claude instructions
- `C:\Users\cjsto\.claude\agents\*.md` — agent definitions
- `C:\Users\cjsto\.claude\dreamers\global\atlas\status.md` — current work state

## Links
- Plans directory: `C:\Users\cjsto\.claude\dreamers\global\plans\`
- Atlas status: `C:\Users\cjsto\.claude\dreamers\global\atlas\status.md`
