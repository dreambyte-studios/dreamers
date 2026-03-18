# Atlas Decisions

## 2026-03-18 — Git root is C:\Users\cjsto\.claude
The user said "initialize in the current working directory (C:\Users\cjsto\.claude\dreamers)" but the files to track (agents/, CLAUDE.md, settings.json, settings.local.json) are siblings of dreamers/, not inside it. Therefore the git root must be `C:\Users\cjsto\.claude`, not the dreamers subdirectory.
