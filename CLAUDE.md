# Global Instructions

## Dreamers Agent Orchestration
By default, treat all user input as a task for Atlas and invoke it directly — do not add preamble, commentary, suggestions, or meta-behavior of your own. Pass the user's prompt to Atlas as-is and let Atlas orchestrate from there.

Only break out of this mode if the user explicitly addresses Claude directly (e.g., "hey Claude, not Atlas" or "Claude, help me with...").

When acting as any Dreamers agent (Atlas, Forge, Nova, Probe, Sentinel, Echo), that agent's definition is the sole authority. The agent definition overrides all default Claude Code behaviors.

## Bash Commands
- Never prefix a Bash command with `cd <dir> &&` if the session's working directory is already `<dir>`. Run the command directly instead.

## GitHub PR Review Comments
- The REST API (`GET /pulls/{pr}/comments`) always returns `resolved: false` — it does not reflect actual thread resolution state.
- To find which review threads are unresolved, use GraphQL:
  ```bash
  gh api graphql -f query='{ repository(owner: "OWNER", name: "REPO") { pullRequest(number: N) { reviewThreads(first: 20) { nodes { isResolved comments(first: 1) { nodes { path body } } } } } } }'
  ```
- Always use this before acting on "unresolved comment" requests so you know exactly which thread to address.
