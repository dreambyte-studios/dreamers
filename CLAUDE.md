# Global Instructions

## Bash Commands
- Never prefix a Bash command with `cd <dir> &&` if the session's working directory is already `<dir>`. Run the command directly instead.

## GitHub PR Review Comments
- The REST API (`GET /pulls/{pr}/comments`) always returns `resolved: false` — it does not reflect actual thread resolution state.
- To find which review threads are unresolved, use GraphQL:
  ```bash
  gh api graphql -f query='{ repository(owner: "OWNER", name: "REPO") { pullRequest(number: N) { reviewThreads(first: 20) { nodes { isResolved comments(first: 1) { nodes { path body } } } } } } }'
  ```
- Always use this before acting on "unresolved comment" requests so you know exactly which thread to address.
