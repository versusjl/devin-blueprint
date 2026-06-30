---
name: pr-to-ready
description: "Drive an open pull request to merge-ready by inspecting live PR state, classifying feedback, fixing actionable findings, verifying checks, and reporting merge readiness. Never merges. Use when the user asks 'fix the PR', 'address review comments', 'merge?', 'is this ready?', or wants PR feedback resolved."
user-invocable: true
argument-hint: "<PR URL, number, branch, or current repository PR>"
---

# PR To Ready

Drive an open pull request to merge-ready. The job is independent validation, not agreement: re-read the live PR state and decide from evidence. Merging is always a human decision. Devin has the final say on whether a PR is ready, but never merges it.

## Workflow

1. Identify the PR from `$ARGUMENTS`, the current branch, or the repository's open PRs.
2. Inspect live state:
   - PR title, body, base/head branches, mergeability, changed files, and latest commits
   - check runs, required statuses, and failing or pending jobs
   - review submissions, unresolved threads, top-level comments, and bot comments (including Devin Review)
   - local working tree status
3. Classify feedback before editing:
   - `actionable`: still applies and should be fixed
   - `resolved`: already fixed or answered
   - `outdated`: attached to old code or obsolete context
   - `informational`: useful but not required for merge
   - `needs-human`: requires product, security, ownership, or risk acceptance
4. Patch only actionable findings. Keep changes narrow and consistent with the repo. If a bot comment (e.g. Devin Review) flags something whose fix would reverse or contradict an explicit user instruction, escalate to the user rather than acting on it.
5. Run the smallest verification that proves the fixes, then broader tests when shared behavior, public APIs, security, or user-visible flows changed. For UI changes, use `browser-verify` to validate the rendered output.
6. Re-check live PR state after pushing.
7. Sync the linked Linear ticket when the PR references one: keep it in the existing review state, and comment the readiness verdict with evidence -- fixes made, checks passing, verification run. Use existing states only; do not invent states or labels.
8. Report merge readiness with evidence.

## When Devin handles pr-to-ready directly

- the PR touches high-risk, security-sensitive, or architecturally significant code
- the feedback requires judgment to classify (actionable vs outdated vs informational)
- the PR is Devin's own work and Devin is the final quality gate

## When Devin delegates parts of pr-to-ready

- a specific fix is structurally clear and can be delegated to Claude or Codex
- CI failures point to a narrow, well-bounded issue

Devin still reviews the delegated fix and re-inspects the live PR state before reporting readiness.

## Rules

- Never merge the PR. The skill ends at a merge-readiness verdict; a human merges.
- Do not rely on stale chat summaries. Inspect the PR again.
- Do not treat a resolved or outdated bot comment as current work.
- Do not mark a PR ready while required checks are failing or pending.
- Do not broaden scope because a bot suggested a nice-to-have refactor.
- Do not hide placeholders, skipped tests, partial fixes, or assumptions.
- Stop for human input if a finding requires authority the agent does not have.
- Update linked Linear tickets only through existing states and comments.
- If you cannot access the PR host, say so and fall back to local branch, diff, and test evidence only.
- If something breaks during the fix, self-anneal: fix it, test it, and write the learning into Knowledge or the Playbook.

## Report

Lead with the decision:

- **Ready**: all actionable feedback is resolved and required checks pass.
- **Not ready**: blockers remain.
- **Blocked**: missing access, missing environment, or human decision required.

Then include:

- PR inspected: number or URL
- Live state checked: reviews, threads/comments, checks, mergeability, working tree
- Changes made: concise bullets with files or areas
- Verification: commands run and results (including browser-verify evidence if applicable)
- Remaining items: blockers, pending checks, human decisions, or risks

## Merge answer

When the user asks "merge?", answer directly, but do not merge:

- "Ready to merge" only when live evidence supports it.
- "Not yet" when checks are pending/failing, actionable feedback remains, or the fix is only partially verified.
- "I can't validate merge readiness" when PR state or required environment is unavailable.