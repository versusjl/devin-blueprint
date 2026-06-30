# Delegate

How Devin routes work to the right agent in the fleet.

## When to use

Use this playbook whenever a task should be handled by another agent instead of Devin directly. This is the Orchestrate layer of the CORE loop.

## Inputs

- The task to delegate (Linear ticket, charter section, or request)
- The target agent (Claude Code, Codex CLI, Devin Local, or custom)

## Steps

1. **Confirm the task is delegation-ready.** It should be structurally clear, have well-defined inputs and outputs, and be independently verifiable. If it's ambiguous, run `triage` first.

2. **Choose the agent.**
   - Claude Code: structured implementation, security review, correctness review
   - Codex CLI: automation-triggered tasks, parallelized ticket work
   - Devin Local (Kimi, GLM): private work, cost-sensitive iteration, offline tasks
   - Devin Cloud (child session): heavy work that needs a full Devin environment

3. **Write the delegation brief.** Include:
   - What to do (clear, scoped instruction)
   - Input context (files, specs, charter references, Linear ticket)
   - Expected output (PR, report, test results, artifact)
   - Acceptance criteria (how Devin will verify the result)
   - Constraints (do not touch X, stay within Y, no file over 300 LOC)

4. **Dispatch.**
   - Linear: create or update the ticket with the brief, assign to the target agent, set label
   - ACP: send via the host-agent wire
   - Child session: use Devin's child session management
   - CLI: invoke Codex or Claude Code with the brief

5. **Monitor.** Check the agent's progress. If it gets stuck or produces unexpected output, intervene or reassign.

6. **Verify.** When the delegated work is returned, Devin reviews it using the `review` skill. Devin has final say on whether the work is ready.

## Success criteria

- The delegated task is completed and verified.
- Devin has reviewed the output.
- The Linear ticket is updated with evidence.
- No unreviewed delegated work has been merged.

## Forbidden actions

- Do not delegate ambiguous or high-risk work without a spec or design doc first.
- Do not let delegated work merge without Devin's review.
- Do not delegate and forget -- always monitor and verify.
