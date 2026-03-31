# Dreamers

An AI coding team built to bring dreams to life.

Dreamers is a structured agent system for that turns software tasks into disciplined, multi-agent pipelines. Instead of one monolithic prompt, work flows through specialized agents with defined roles, shared references, and strict quality gates.

## How it works

**Skills** (`/dreamers-*`) are the entry points. Each skill defines a pipeline that orchestrates one or more agents. You invoke a skill, it reads the refs it needs, and routes work through the right agents in sequence.

**Agents** are specialized subagents, each with a single responsibility:

| Agent | Role | Model |
|-------|------|-------|
| **Forge** | Coder — implements changes strictly against a plan | Sonnet |
| **Nova** | Replanner — re-verifies remaining sub-plans between cycles | Opus |
| **Probe** | Tester — derives tests from acceptance criteria, hunts edge cases | Sonnet |
| **Sentinel** | Reviewer — correctness, security, maintainability checks | Sonnet |
| **Echo** | Documentarian — writes docs from completed implementation | Haiku |
| **Bolt** | Runner — executes mechanical tasks (git ops, PRs, builds) | Haiku |

**Refs** are shared reference docs that define protocols (git workflow, planning rules, quality gates, etc.). Agents read them — never write to them.

**Templates** provide starting structures for plans, PRs, issues, and project briefs.

## Available skills

| Skill | What it does |
|-------|-------------|
| `/dreamers-full` | Full pipeline: plan, implement, review, test, document |
| `/dreamers-plan` | Planning only — produce a plan file from a task description |
| `/dreamers-implement` | Implementation only — execute against an existing plan |
| `/dreamers-fix` | Bug triage — routes to the right tier (quick fix vs. full pipeline) |
| `/dreamers-issue` | Create a GitHub issue with structured ACs |
| `/dreamers-new-project` | Bootstrap a new project from scratch (discovery, brief, shell plans) |
| `/dreamers-pr-resolve` | Resolve unresolved PR review comments |
| `/dreamers-clean-work` | Between-milestone maintenance pass |
| `/dreamers-add-logging` | Add production-grade logging to a project |
| `/dreamers-cleanup-comments` | Code comment cleanup pass |

## Repository structure

```
~/.claude/
  agents/          # Agent definitions (frontmatter + instructions)
  commands/        # Skill definitions (slash commands)
  dreamers/
    refs/          # Shared reference protocols
    templates/     # Starter structures for plans, PRs, issues
  CLAUDE.md        # Global instructions (Dreamers kernel, output rules)
```

Agent work happens **repo-local** in `./.dreamers/` within whatever project you're working on. This repo only contains the system definitions — refs, templates, agents, and skills.

## Core principles

- **Markdown-first** — substantive work goes to files, not chat
- **Plan before you build** — non-trivial work requires a plan file
- **Critical thinking mandate** — agents evaluate before executing, push back on flawed ideas
- **Keep context thin** — git history is the archive; prune live files regularly

## Setup

Clone this repo to `~/.claude/` (or symlink the relevant directories). Claude Code will automatically pick up the agents and skills.

```bash
git clone git@github.com:mrStorrs/dreamers.git ~/.claude
```

Requires [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI or IDE extension.
