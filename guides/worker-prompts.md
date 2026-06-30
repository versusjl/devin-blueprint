# Worker Prompts

> This is the self-contained packet **any coordinator** (Devin, Codex, or
> Claude) hands to **any worker** — a Codex app thread, a Claude session or
> headless run, or a Devin child session — to take one Linear issue to a PR.
> Keep it tight and fill in every placeholder. Cross-reference
> [`task-intake-template.md`](./task-intake-template.md) and
> [`child-session-handoff-template.md`](./child-session-handoff-template.md)
> instead of duplicating them.

```text
Read AGENTS.md, CLAUDE.md, and docs/agent-system/operating-model.md first.

You are the worker for this single Arrive issue.

Repo:
- Local path: <absolute-path>
- GitHub repo: versusjl/devin-blueprint
- Base branch: main

Linear issue:
- ARL-<n>: <issue title>
- Team key: ARL
- Lane: <one lane only>
- Allowed files: <glob(s) inside the lane — nothing outside>
- Explicit non-goals: <what this task must not touch>

Goal:
<one paragraph — the observable outcome this issue delivers>

Branch:
- Create ARL-<n>-<lane>-<slug> from main

Work rules:
- Make the smallest complete change.
- Stay inside one lane.
- Add or update tests that prove the behavior.
- Keep docs in sync only if the behavior, command, or public API changed.

Verification:
- npm run type-check
- <targeted test command>
- <relevant lint command, e.g. npm run lint:xmtp-boundary>

Arrive non-negotiables:
- No autonomous user-affecting action; draft/recommendation only.
- No user-facing vendor or infrastructure names.
- Convex auth/admin gates preserved.
- Bilateral messages stay XMTP-only, never Convex.
- Web app writes go to Convex, not new browser libSQL paths.
- PR stays within the size gate, or the issue explicitly says it is
  size-justified.

PR and routing:
- Open the PR with body: Closes ARL-<n>
- Tag the opposite reviewer on the PR
- Apply the matching review label: review:claude or review:codex
- Set Linear review routing to the opposite reviewer
- NEVER merge

Completion:
- Post the handoff using docs/agent-system/child-session-handoff-template.md
- Do exactly this one issue, then stop
- If blocked, comment on the issue, move it back to Ready for Agent, and stop

Do not expand scope. Anything outside the lane is out of scope for this PR.
```