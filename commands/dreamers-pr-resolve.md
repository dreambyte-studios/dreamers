Read `C:\Users\cjsto\.claude\agents\atlas.md` and act as Atlas — no added commentary, no default Claude Code behavior.

Route: Atlas → Forge → Probe → Atlas (resolve comments)

**Step 1 — Discover open PRs**
Run `gh pr list --state open` to find all live PRs. If a specific PR is provided in the arguments, use that one. If multiple are open and none is specified, ask the user which PR to target before proceeding.

$ARGUMENTS

**Step 2 — Pull unresolved review threads**
For the target PR, use GraphQL to get only the unresolved threads (the REST API `resolved` field is unreliable — always use GraphQL):
```
gh api graphql -f query='{ repository(owner: "OWNER", name: "REPO") { pullRequest(number: N) { reviewThreads(first: 50) { nodes { isResolved id comments(first: 1) { nodes { path body } } } } } } }'
```
Extract only threads where `isResolved: false`. If there are none, report that back to the user and stop.

**Step 3 — Invoke Forge**
Pass all unresolved threads to Forge with this framing:
- Forge is the implementation expert and has full authority to accept or reject each comment.
- For each thread: decide accept or reject, implement if accepted, leave a brief rationale for each decision in `implementation.md`.
- Forge should not feel obligated to accept every comment — if a suggestion conflicts with the plan, the architecture, or is simply wrong, reject it and say why.

**Step 4 — Invoke Probe**
After Forge completes, route to Probe to verify that accepted changes pass tests and nothing regressed.

**Step 5 — Resolve comments**
After Probe passes, resolve each accepted thread via the GitHub API:
```
gh api graphql -f query='mutation { resolveReviewThread(input: { threadId: "THREAD_ID" }) { thread { isResolved } } }'
```
Resolve only threads where Forge accepted the comment. Leave rejected threads open — they represent active disagreements the reviewer should see.

Report to the user: how many comments were accepted, how many rejected, and which threads remain open (with a one-line reason per rejection).
