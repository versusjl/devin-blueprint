# Knowledge Update

How to propose a new or updated Knowledge note from session findings.

## When to use

Use this playbook when you discover a standing convention, recurring pattern, gotcha, or constraint that should persist across sessions.

## Inputs

- The finding or learning to capture
- The context where it applies (repo, tool, workflow, API)

## Steps

1. **Check existing Knowledge.** Search `knowledge/` in the repo and Devin's Knowledge store for existing notes on the same topic. If one exists, draft an update instead of a new note.

2. **Draft the note.** Write it in `knowledge/<topic-slug>.md` with:
   - A clear title
   - A semantic trigger description (when should Devin recall this?)
   - The convention, constraint, or pattern in plain language
   - Examples when they help
   - Source (which session, ticket, or incident produced this learning)

3. **Keep it small.** One note per topic. If the note covers multiple unrelated things, split it.

4. **Propose for review.** Do not save directly. Submit the draft for human review. If accepted, copy into Devin's Knowledge store with the same trigger description.

5. **Version.** If updating an existing note, increment the version and note what changed.

## Success criteria

- The Knowledge note is small, single-purpose, and has a clear semantic trigger.
- It exists in both `knowledge/` (repo canonical) and Devin's Knowledge store.
- It was reviewed before saving.

## Forbidden actions

- Do not silently overwrite existing Knowledge notes.
- Do not create notes for one-off findings that will never recur.
- Do not duplicate information that already lives in a Charter or Playbook.