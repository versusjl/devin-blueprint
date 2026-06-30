---
name: tdd
description: "Test-first variant of implement: understand the desired behavior, write a failing test, make it pass, then simplify."
user-invocable: true
argument-hint: "<task reference or behavior> e.g. 'LIN-123' or 'retry logic for API client'"
---

# Test-Driven Development

Use this for behavioral changes where a failing test can describe the desired outcome before implementation.

## Workflow

### 1. Understand

- Read the request, Linear ticket, Charter, plan, spec, and relevant code as available.
- Identify the desired behavior, existing contracts, failure paths, and verification.
- If a spec exists, note its invariants, decisions, and testing strategy.
- Ask before writing tests when missing information would materially change behavior, scope, safety, contracts, data shape, or verification.

### 2. Red

- Write the smallest failing test that proves the desired behavior or reproduces the bug.
- Run it and confirm it fails for the expected reason.

### 3. Green

- Write the minimum implementation needed to pass the test.
- Preserve existing contracts unless the task explicitly changes them.
- Add failure-path tests where they matter.
- No source file should exceed 300 LOC. If the implementation pushes a file past the limit, split before continuing.

### 4. Refine

- Simplify code and tests while they stay green.
- Run the full relevant test suite before finishing.
- For browser-rendered work, use `browser-verify` to confirm the output.
- Report the failing-then-passing test and final verification.
- If the task came from Linear, update the ticket with the evidence.

## Rules

- Do not write implementation code before a failing test for the behavior.
- If an assumption is low-risk, make it explicit and keep moving.
- Tests describe behavior, not implementation details.
- Prefer real boundaries over mocks when practical.
- Skip TDD for documentation, formatting, or non-behavioral scaffolding work.
- If implementation reveals the instructions are wrong, stop. Update the task, spec, or plan first.