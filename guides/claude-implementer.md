
# Claude Code Coordinator Loop

> **Read first:** [`AGENTS.md`](../../AGENTS.md), [`CLAUDE.md`](../../CLAUDE.md), and [`operating-model.md`](./operating-model.md). This file does **not** restate the shared rules — it only describes how **Claude Code** runs the coordinator role. The shared rules win on any conflict.

The coordinator is **whoever starts the work**. When you open a Claude Code
run and tell it to work through a set of issues, Claude Code is the coordinator
for that run. This guide is how Claude Code fills that role using its own
sessions, scheduled triggers, and headless `claude -p` runs. Workers are pulled
from [`worker-prompts.md`](./worker-prompts.md); the lanes, four-gate review,
and `ARL` sync are the shared ones from `operating-model.md`.

Use this when you want Claude Code to coordinate a bounded issue set, dispatch
lane-disjoint work to workers, and keep Claude focused on orchestration and
review rather than routine implementation. This matches
[`autonomous-loops.md`](./autonomous-loops.md) Options 2/4 and
[`claude-web-overnight-loop.md`](./claude-web-overnight-loop.md) without
repeating them.

This loop is useful when:

- the issue set is finite and ordered;
- Linear should stay current (team key `ARL`);
- each issue should become its own branch and PR;
- Claude should coordinate through its own sessions or headless runs;
- Claude should hand each worker the packet from `worker-prompts.md`;
- Claude should do the adversarial review before PR;
- Claude should never self-merge.

Do not use this as a normal implementation loop. Claude coordinates, routes,
reviews, and hands off. Workers implement.

## Coordinator Prompt

```text
Create a goal for this run:

Work through <issue-list> in <repo> and turn them into PRs, one issue at a
time, using a coordinator workflow.

Repository:
- Local path: <absolute-path>
- GitHub repo: versusjl/devin-blueprint
- Remote: <remote-url>
- Base branch: main

Source of truth:
- The listed Linear issues (team key ARL)
- AGENTS.md, CLAUDE.md, and docs/agent-system/operating-model.md
- Any linked PRD or plan doc
- The Linear board (states below)

Claude coordinator policy:
- Claude coordinates the run.
- Implementation work must happen in separate Claude worker sessions or fresh
  headless runs.
- Before creating a worker, pull the issue, inspect the repo state, and write a
  short plan if the task is non-trivial.
- For each active issue, spawn a fresh worker session with the self-contained
  packet from docs/agent-system/worker-prompts.md, filled in with the issue,
  repo context, branch name, acceptance criteria, verification steps, Linear
  rules, PR expectations, and reporting requirements.
- Use lane-disjoint issues only.
- Keep Claude focused on orchestration, review, and integration — not routine
  implementation.

Linear tracking (team key ARL):
- Move an issue to In Progress when active work starts.
- Keep it In Progress through planning, coding, review, PR, and checks.
- Move it to In Review when the PR is open; on merge the magic word auto-moves
  it to Done.
- If blocked after work starts, leave it In Progress and comment the blocker on
  the issue.
- If work never starts, leave it in Todo / Ready for Agent.

Workflow for each issue:

1. Pull the ticket.
- Read the issue, linked context, AGENTS.md, CLAUDE.md, and relevant repo docs.
- Inspect the current repo state.
- Decide whether the issue is unblocked. If blocked, stop and explain.
- If unblocked and active work is starting, move the Linear issue to In
  Progress.

2. Plan the work.
- For simple issues, write a short plan in the coordinator thread.
- For non-trivial issues, write a plan under docs/issues/ARL-<n>-plan.md.
- Include goal, context, likely files, acceptance criteria, verification, and
  out of scope.

3. Spawn a worker session and isolate.
- Create a separate worker session per issue.
- Prefer a worktree starting from main for each worker.
- One branch per issue. Branch naming: ARL-<n>-<lane>-<slug>.
- Stay inside one lane. Do not stage unrelated changes.
- Hand the worker the filled packet from docs/agent-system/worker-prompts.md.

4. Implement.
- The worker makes the smallest complete change that satisfies the issue.
- Follow existing project patterns; add/update tests that catch the behavior.
- Keep docs aligned when behavior, commands, public APIs, or decisions change.

5. Verify.
- Run the issue's listed verification commands and the relevant test suite.
- Run npm run type-check. For user-visible or terminal behavior, run the manual
  smoke checks.
- Record anything that could not be verified.

6. Adversarial review.
- Inside the worker session, use a fresh internal reviewer to challenge the
  branch before PR.
- Check correctness, acceptance criteria, missing tests, edge cases, and repo
  rules.

7. Open the PR.
- Open a PR with body `Closes ARL-<n>`.
- Tag the opposite reviewer, apply the matching review label, and set Linear
  review routing.
- Do not merge.

8. Final gate.
- Claude hands the branch to Devin for the final Sr. review after peer review
  and before the human gate.
- Humans merge.

If anything is ambiguous or blocked, leave a Linear comment explaining the
blocker, move it back to `Ready for Agent` if it never started, and stop instead
of guessing.
```