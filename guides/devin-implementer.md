# Devin Coordinator Loop

> **Read first:** [`AGENTS.md`](../AGENTS.md), [`CLAUDE.md`](../CLAUDE.md),
> [`loops.md`](./loops.md), and [`labels.md`](./labels.md). This file does
> **not** restate the shared rules — it only describes how **Devin** runs the
> coordinator role. The shared rules win on any conflict.

The coordinator is **whoever starts the work**. When Devin opens the run and
starts decomposing or dispatching issues, Devin is the coordinator for that
run. This guide is how Devin fills that role with **child sessions**,
**scheduled sessions**, and **Automations**. Workers are pulled from
[`worker-prompts.md`](./worker-prompts.md); the lanes, review gates, and
Linear sync are the shared ones from `loops.md` and `labels.md`.

Use this when you want Devin to orchestrate a bounded issue set, dispatch
lane-disjoint work to workers, and keep Devin focused on orchestration and final
Sr. review rather than routine implementation. Reserve Devin ACUs for
orchestration and Sr. review — not for everyday coding.

This loop is useful when:

- the issue set is finite and ordered;
- Linear should stay current;
- each issue should become its own branch and PR;
- Devin should own PRD → Linear decomposition via
  [`orchestrator.md`](../playbooks/orchestrator.md);
- the coordinator should spawn a worker child session per lane-disjoint issue;
- the coordinator may mix Devin child sessions with Claude Code or Codex
  workers;
- Devin should do the final Sr. review before the human gate;
- self-merge is never allowed.

Do not use this as a normal implementation loop. Devin coordinates, routes,
reviews, and hands off. Workers implement.

## Coordinator Prompt

```text
Create a goal for this run:

Work through <issue-list> in <repo> and turn them into PRs, one issue at a
time, using a coordinator workflow.

Repository:
- Local path: <absolute-path>
- GitHub repo: <owner>/<repo>
- Remote: <remote-url>
- Base branch: main

Source of truth:
- The listed Linear issues (team key <TEAM>)
- AGENTS.md and CLAUDE.md
- Any linked PRD or plan doc
- The Linear board (states below)

Devin coordinator policy:
- Devin coordinates the run.
- Implementation work must happen in separate worker child sessions.
- Before creating a worker, pull the issue, inspect the repo state, and write a
  short plan if the task is non-trivial.
- For each active issue, spawn a fresh worker child session with the
  self-contained packet from guides/worker-prompts.md, filled in with
  the issue, repo context, branch name, acceptance criteria, verification
  steps, Linear rules, PR expectations, and reporting requirements.
- Use lane-disjoint issues only.
- Keep Devin focused on orchestration, review, and integration — not routine
  implementation.

Linear tracking (team key <TEAM>):
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
- For non-trivial issues, write a plan under docs/issues/<TEAM>-<n>-plan.md.
- Include goal, context, likely files, acceptance criteria, verification, and
  out of scope.

3. Spawn a worker child session and isolate.
- Create a separate worker child session per issue.
- Prefer a worktree starting from main for each worker.
- One branch per issue. Branch naming: <TEAM>-<n>-<lane>-<slug>.
- Stay inside one lane. Do not stage unrelated changes.
- Hand the worker the filled packet from guides/worker-prompts.md.

4. Implement.
- The worker makes the smallest complete change that satisfies the issue.
- Follow existing project patterns; add/update tests that catch the behavior.
- Keep docs aligned when behavior, commands, public APIs, or decisions change.

5. Verify.
- Run the issue's listed verification commands and the relevant test suite.
- Run the project's type-check. For user-visible or terminal behavior, run the
  manual smoke checks.
- Record anything that could not be verified.

6. Adversarial review.
- Inside the worker thread, use a fresh internal reviewer to challenge the
  branch before PR.
- Check correctness, acceptance criteria, missing tests, edge cases, and repo
  rules.

7. Open the PR.
- Open a PR with body `Closes <TEAM>-<n>`.
- Tag the opposite reviewer, apply the matching review label, and set Linear
  review routing.
- Do not merge.

8. Final gate.
- Devin performs the final Sr. review after peer review and before the human
  gate.
- Humans merge.

If anything is ambiguous or blocked, leave a Linear comment explaining the
blocker, move it back to `Ready for Agent` if it never started, and stop instead
of guessing.
```
