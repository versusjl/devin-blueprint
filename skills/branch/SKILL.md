---
name: branch
description: "Create a traceable Git branch for the current task."
user-invocable: true
argument-hint: "<branch-name or task reference>"
---

# Branch

Create a traceable Git branch for the current task.

## Workflow

1. Inspect the current Git branch, `git status`, and available task context -- including Linear tickets, Charters, and session state.
2. Identify the ticket ID when the work comes from Linear or another issue tracker. If the task has a ticket but no visible ID, ask for it.
3. Use the user's branch name if provided. If work from Linear omits the ticket ID, ask before creating the branch.
4. Otherwise derive the branch name:
   - `<ticket-id>-<short-kebab-summary>` when there is a ticket ID (e.g. `ARL-123-add-retry-logic`)
   - `devin/<timestamp>-<short-kebab-summary>` when there is no ticket ID
5. Preserve an obvious repo prefix such as `feature/` only if the ticket ID remains visible.
6. If the branch already exists, switch to it. Otherwise, create and switch to it.
7. Report the branch name and any uncommitted work that was already present.

## Rules

- Stop if the working tree state makes switching branches unsafe.
- Do not overwrite or discard uncommitted work.
- Include the ticket ID for work from Linear or any issue tracker.
- Do not invent ticket IDs.
- Never force push to `main` or `master`.
- Never amend existing commits -- only add new commits.
- Never skip hooks (`--no-verify`) unless the user explicitly requests it.