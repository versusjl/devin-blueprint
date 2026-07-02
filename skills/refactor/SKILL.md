---
name: refactor
description: "Improve the shape of existing code without changing behavior. Enforce a 300 LOC per-file maximum."
user-invocable: true
argument-hint: "[optional files, diff, commit, or cleanup focus]"
---

# Refactor

Improve code shape without changing behavior. Use this when the user asks to refactor, simplify, tidy, clean up, reduce duplication, improve design, or make code easier to maintain.

## Workflow

### 1. Understand

- Use `$ARGUMENTS`, specified files, `git diff`, staged changes, or the latest commit.
- If staged changes exist, review `git diff HEAD`; otherwise review `git diff`.
- If there are no Git changes, review the most recently modified files named by the user or touched in the current task.
- Read the target code, surrounding patterns, tests, contracts, invariants, and verification commands.
- Check Knowledge notes and Playbooks for prior refactoring patterns or conventions in this repo.
- Identify what behavior must stay the same before editing.
- For risky refactors, run existing focused tests first to establish the current baseline.

### 2. Find simplifications

Look for changes that would make the code easier to understand, maintain, or safely extend:

- **Files over 300 lines of code** -- any source file exceeding 300 LOC should be split. This is a hard ceiling, not a suggestion.
- duplicated or near-duplicated logic
- unclear names or hidden concepts
- long functions doing multiple jobs
- unnecessary abstractions, wrappers, layers, or indirection
- missing reuse of existing helpers, utilities, scripts in `run/`, or local conventions
- tangled conditionals, data flow, or error handling
- poor module boundaries or leaky abstractions
- comments that could become clearer code
- inefficient work that can be removed without changing behavior

### 3. Refactor

- Make small, targeted, behavior-preserving edits.
- **No source file should exceed 300 lines of code.** When a file is over the limit, split it into focused modules with clear boundaries. Extract by responsibility, not by arbitrary line count.
- Prefer deleting code, clarifying names, extracting or inlining functions, reusing existing helpers, and simplifying control flow.
- Add an abstraction only when it clearly reduces complexity or meaningful duplication.
- Preserve public contracts, data shape, error behavior, and user-visible output unless the user explicitly asks to change them.
- Skip changes that would make the code merely different rather than simpler.
- If a script belongs in `run/`, move it there: deterministic, testable, one job, no reasoning.

### 4. Verify

- Run the relevant tests and checks after editing.
- Run broader tests when the refactor touches shared behavior or contracts.
- For browser-rendered work, use `browser-verify` to confirm nothing visual broke.
- If no code changes were needed, say the target was already clean.
- Report what changed, what stayed behaviorally the same, and what verification ran.

## When Devin refactors directly

- the refactor requires understanding cross-cutting concerns or architectural boundaries
- the file split decisions need judgment about module responsibility
- the code touches high-risk or security-sensitive areas

## When Devin delegates refactoring

- the refactor is mechanical (rename, extract a well-bounded function, move a file)
- the pattern is clear and repeatable across many files
- the work can be parallelized across agents via a coordinator loop

Devin still reviews delegated refactors before they move forward.

## The 300 LOC rule

Every source file should be 300 lines of code or fewer. This is not about aesthetics -- it is about maintainability, reviewability, and agent effectiveness:

- Shorter files are easier for agents to hold in context.
- Shorter files produce smaller, more focused diffs.
- Shorter files are easier to test, review, and reason about.
- When a file crosses 300 LOC, split it by responsibility into focused modules.

If splitting a file would break public contracts or require coordinated changes across many consumers, flag it and propose a plan before proceeding.

## Rules

- Refactoring is not feature work.
- Do not broaden scope into unrelated cleanup.
- Do not change behavior to make refactoring easier.
- Stop and ask before changing public contracts, data shape, or user-visible behavior.
- Keep abstractions earned by real simplification, not aesthetic preference.
- A good refactor makes the code easier to understand, not merely different.
- No source file should exceed 300 lines of code.
- If something breaks during refactoring, self-anneal: fix it, test it, and write the learning into Knowledge or the Playbook.