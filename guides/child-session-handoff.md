# Worker Handoff Template

> Every Worker (Claude / Codex / Devin) posts this as a comment on the issue/PR when
> it finishes a task. It is how work is "handed off" — the next agent or the reviewer
> reads this, not a live conversation. Keep it factual; no scope expansion.

```md
## Worker Handoff

Task ID:        <#issue>
Lane:           <lane>
Agent:          claude | codex | devin
PR:             <link>

Scope completed:
<what was actually built, in 1–3 lines>

Files changed:
- path/to/file — what changed
- path/to/file.test — tests added

Commands run (with result):
- npm run type-check        → pass/fail
- npm run test:<scope>      → N passed
- npm run lint:<relevant>   → clean

Acceptance criteria:
- [x] <criterion 1>
- [x] <criterion 2>

Known risks:
- <risk, or "none">

Open questions (for reviewer / human):
- <question, or "none">

Out-of-scope discoveries (DO NOT fix here — file separately):
- <thing found outside the lane/scope → becomes a new task>

Unblocks:
- <#issue this now lets start, or "nothing">
```

### Rules
- **Do not fix out-of-scope discoveries in this PR.** List them; they become new tasks.
- Every changed file must be inside the task's lane. If not, the handoff is rejected.
- "Commands run" must show real output status, not "should pass."