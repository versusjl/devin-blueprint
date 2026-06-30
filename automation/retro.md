# Retro

Automated post-session retrospective. Runs after every completed Devin session.

## Trigger

Session completion (any Devin session that produced code changes, PRs, or task updates).

## Steps

1. **Read Session Insights.** Pull the session analysis: what was attempted, what succeeded, what failed, what was surprising.

2. **Classify findings.** For each finding, determine what type of update it suggests:
   - Charter update: the project scope, goals, or constraints should change
   - Knowledge note: a standing convention, gotcha, or pattern was discovered
   - Playbook step: a repeatable procedure should be added or modified
   - Forbidden Action: something should never be done again
   - No action: the finding is informational only

3. **Draft updates.** For each actionable finding, draft the specific update:
   - Charter: write the proposed change with version bump
   - Knowledge: draft the note with semantic trigger
   - Playbook: draft the new or modified step
   - Forbidden Action: state what is forbidden and why

4. **Propose.** Submit all proposed updates for human review. Do not silently overwrite any artifact.

5. **Report.** Post a summary to the Linear ticket or session thread:
   - What worked
   - What broke or surprised
   - Proposed updates (with links)
   - Lessons learned

## Success criteria

- Every session with meaningful findings produces at least one proposed update.
- No artifact is overwritten without approval.
- The system is measurably stronger after each retro.