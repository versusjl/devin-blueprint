# Worker Prompts

> This is the self-contained packet **any coordinator** (Devin, Codex, or
> Claude) hands to **any worker** — a Codex app thread, a Claude session or
> headless run, or a Devin child session — to take one Linear issue to a PR.
> Keep it tight and fill in every placeholder. Cross-reference
> [`child-session-handoff.md`](./child-session-handoff.md) instead of
> duplicating it.

```text
Read AGENTS.md and CLAUDE.md first.

You are the worker for this single issue.

Repo:
- Local path: <absolute-path>
- GitHub repo: <owner>/<repo>
- Base branch: main

Linear issue:
- <TEAM>-<n>: <issue title>
- Team key: <TEAM>
- Lane: <one lane only>
- Allowed files: <glob(s) inside the lane — nothing outside>
- Explicit non-goals: <what this task must not touch>

Goal:
<one paragraph — the observable outcome this issue delivers>

Branch:
- Create <TEAM>-<n>-<lane>-<slug> from main

Work rules:
- Make the smallest complete change.
- Stay inside one lane.
- Add or update tests that prove the behavior.
- Keep docs in sync only if the behavior, command, or public API changed.

Verification:
- <type-check command>
- <targeted test command>
- <relevant lint command>

Project non-negotiables:
- <the hard rules from AGENTS.md this issue could plausibly violate —
  auth gates, data-boundary rules, size gates, vendor-naming rules>
- PR stays within the size gate, or the issue explicitly says it is
  size-justified.

PR and routing:
- Open the PR with body: Closes <TEAM>-<n>
- Tag the opposite reviewer on the PR
- Apply the matching review label: review:claude or review:codex
- Set Linear review routing to the opposite reviewer
- NEVER merge

Completion:
- Post the handoff using guides/child-session-handoff.md
- Do exactly this one issue, then stop
- If blocked, comment on the issue, move it back to Ready for Agent, and stop

Do not expand scope. Anything outside the lane is out of scope for this PR.
```
