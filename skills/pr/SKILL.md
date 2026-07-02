---
name: pr
description: "Commit the intended changes, push the branch, and open a clear draft pull request."
user-invocable: true
argument-hint: "[optional PR title or ticket reference]"
---

# PR

Turn the current branch's work into a draft pull request.

## Workflow

1. Inspect `git status` and the full diff against the base branch. If there is nothing to propose, stop.
2. Commit intended changes using the `commit` skill conventions. Never stage secrets.
3. Push the branch to the remote.
4. Open a draft PR against the default branch (e.g. `gh pr create --draft`).
5. Write the body: what changed and why, the linked ticket (`Closes <ticket-ID>` when one exists), tests and checks run with results, and known limitations or anything not verified.
6. Report the PR URL.

## Rules

- Draft by default. Never merge, and never mark ready-for-review unless asked.
- The PR body must reflect what was actually run, not what should pass.
- One concern per PR. If the diff mixes unrelated changes, say so instead of shipping it.
- If the push or PR creation fails for permissions, report it and stop; do not work around access controls.
