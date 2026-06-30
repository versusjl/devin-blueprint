# Self-Annealing

> Semantic trigger: when an error occurs during execution, when a script fails, when a tool returns unexpected results, when a workaround is needed.

## The rule

When something breaks during a session, do not just fix it and move on. Fix it, harden the system, and write back what you learned.

## The loop

1. **Read the error.** Understand the stack trace, the failing input, and the root cause.
2. **Fix it.** Repair the script, tool, or inputs. If the fix spends paid credits or tokens, check with the user first.
3. **Harden.** Swap to a better MCP connector, add input validation, handle the edge case, or tighten the error path.
4. **Test.** Confirm the fix works. Run the relevant checks.
5. **Write back.** Capture what you learned into the appropriate artifact:
   - Rate limits, timing constraints, API quirks -> Knowledge note
   - New or modified procedure -> Playbook step
   - Changed project assumptions -> Charter update (propose, don't overwrite)
   - Something that should never happen again -> Forbidden Action

## Examples

- Hit a rate limit -> inspect the API -> find a batch endpoint -> adjust the call -> re-run -> write the constraint into the Playbook.
- Script fails on empty input -> add a guard clause -> test with empty input -> add "always validate inputs" to the Knowledge note for that script.
- MCP tool returns stale data -> switch to direct API call -> test -> update the Playbook to prefer the direct call.

## What not to do

- Do not silently work around errors without writing back.
- Do not overwrite Charters or Playbooks without proposing the change.
- Do not spend paid credits retrying without user approval.