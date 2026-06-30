# Codex Coordinator Loop

> **Read first:** [`AGENTS.md`](../../AGENTS.md), [`CLAUDE.md`](../../CLAUDE.md) (data-layer
> rules), and [`operating-model.md`](./operating-model.md). This file does **not** restate
> the lanes, review gates, or Linear sync rules — it only describes how **Codex** runs the
> coordinator role. The shared rules win on any conflict.

The coordinator is **whoever starts the work**. When you open a Codex thread and tell it to
work through a set of issues, Codex is the coordinator for that run. This guide is how Codex
fills that role using its own app threads and worktrees. Workers are picked from
[`worker-prompts.md`](./worker-prompts.md); the lanes, four-gate review, and `ARL` sync are
the shared ones from `operating-model.md`.

Use this when you want one attended Codex thread to work through a small, explicit issue set.
It is a prompt pattern, not a skill. The skills still do the judgment: `task-to-pr`,
`implement`, `review`, `pr`, and `pr-to-ready`.

This loop is useful when:

- the issue set is finite and ordered;
- Linear should stay current (team key `ARL`);
- each issue should become its own branch and PR;
- the coordinator should run an adversarial review before PR;
- merging is explicitly authorized for this run.

Do not use this as the default unattended loop. The default remains draft PRs for human
review (four gates: CodeRabbit/Greptile → opposite-agent peer review → Devin Sr. review →
human merge — see `operating-model.md` §4).

## Coordinator Prompt

```text
Create a goal for this thread:

Work through <issue-list> in <repo> and turn them into merged PRs, one issue at a time,
using a coordinator workflow.

Repository:
- Local path: <absolute-path>
- GitHub repo: meetarrivexyz/getarrive
- Remote: <remote-url>
- Base branch: main

Source of truth:
- The listed Linear issues (team key ARL)
- AGENTS.md, CLAUDE.md, and docs/agent-system/operating-model.md
- Any linked PRD in docs/prd/ or plan doc
- The Linear board (states below)

Codex thread policy:
- The coordinator thread coordinates.
- Implementation work must happen in separate Codex app threads, not internal subagents.
- Do not implement issues directly in the coordinator thread unless `codex_app.create_thread`
  is unavailable or fails.
- Before creating a worker thread, call `codex_app.list_projects` and find the project for
  this repo.
- For each active issue lane, call `codex_app.create_thread` with a project target and a
  worktree environment.
- Prefer a worktree starting from main for each issue worker.
- Give each worker the self-contained prompt from docs/agent-system/worker-prompts.md, filled
  in with the issue, repo context, branch name, acceptance criteria, verification steps,
  Linear rules, PR expectations, and reporting requirements.
- Use internal subagents only for adversarial review inside a worker thread.
- Do not use an internal subagent as a substitute for an implementation worker thread.

Linear tracking (team key ARL):
- Move an issue to In Progress when active work starts.
- Keep it In Progress through planning, coding, review, PR, and checks.
- Move it to In Review when the PR is open; on merge the magic word auto-moves it to Done.
- If blocked after work starts, leave it In Progress and comment the blocker on the issue.
- If work never starts, leave it in Todo / Ready for Agent.

Workflow for each issue:

1. Pull the ticket.
- Read the issue, linked context, AGENTS.md, and relevant repo docs.
- Inspect the current repo state.
- Decide whether the issue is unblocked. If blocked, stop and explain.
- If unblocked and active work is starting, move the Linear issue to In Progress.

2. Plan the work.
- For simple issues, write a short plan in the thread.
- For non-trivial issues, write a plan under docs/issues/ARL-<n>-plan.md.
- Include goal, context, likely files, acceptance criteria, verification, and out of scope.

3. Create worker thread, branch, and isolate.
- Create a separate Codex app worker thread per issue (`codex_app.create_thread`).
- Resolve the project ID first with `codex_app.list_projects`.
- Use a project target with a worktree environment.
- One branch per issue. Branch naming: ARL-<n>-<lane>-<slug>.
- Stay inside one lane (operating-model.md §3). Do not stage unrelated changes.

4. Implement.
- The worker makes the smallest complete change that satisfies the issue.
- Follow existing project patterns; add/update tests that catch the behavior.
- Keep docs aligned when behavior, commands, public APIs, or decisions change.

5. Verify.
- Run the issue's listed verification commands and the relevant test suite.
- Run npm run type-check. For user-visible/terminal behavior, run the manual smoke checks.
- Record anything that could not be verified.

6. Adversarial review.
- Inside the worker thread, use a fresh internal subagent to review the branch before PR.
- Check correctness, acceptance criteria, missing tests, edge cases, regressions,
  maintainability, over-broad scope, and the Arrive non-negotiables (AGENTS.md).
- Judge every finding; fix valid in-scope ones; rerun relevant checks.

7. Open PR.
- Commit only intended changes; push the branch; open a PR against main.
- PR body MUST contain "Closes ARL-<n>" so Linear links and auto-moves on merge.
- Include linked issue, summary, acceptance checklist, tests run, manual verification,
  review summary, and known limitations.
- Tag the opposite reviewer (`@claude please review this PR`), apply the `review:claude`
  label, and set the Linear review routing field to claude-review.

8. Merge (only if this prompt explicitly authorizes it).
- Merge only because this prompt explicitly authorizes it — otherwise stop at the human gate.
- Note: branch protection requires CODEOWNERS / human approval, so an unattended thread
  cannot self-merge. Authorized merge means an attended run where Jason has cleared it.
- Before merging, ensure local tests pass and remote checks pass.
- Use the repo's documented merge policy; if none, squash merge.
- After merge, confirm Linear auto-moved the issue to Done; update local main.

Execution:
- Process issues in the listed order.
- Parallelize only when issues are genuinely independent and their lanes are disjoint.
- Prefer clean sequencing over speed when issues share files, decisions, or assumptions.

Stop conditions:
- Acceptance criteria unclear; tracker/push/PR/merge permissions fail; a design decision
  needs human approval; tests reveal a deeper problem outside the issue scope; or a task
  would violate an Arrive non-negotiable (AGENTS.md).

Report after each issue:
- Issue ID and title; Linear status; branch; worktree path (if used); PR URL; merge result;
  tests run; review findings fixed; anything deferred. Post the worker handoff
  (docs/agent-system/child-session-handoff-template.md).

Begin with the first issue.
```

## Notes

This loop deliberately extends beyond `task-to-pr`. It owns sequencing and, when explicitly
authorized, merge. Use it for attended project pushes, not unattended background work — the
unattended Codex worker loop is the hourly Automation in
[`worker-cron-automation-prd.md`](./worker-cron-automation-prd.md).