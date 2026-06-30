# Claude Code

Read [AGENTS.md](AGENTS.md) first. This file contains Claude Code-specific adapter notes only.

## Skills

Blueprint workflows live in `skills/<name>/SKILL.md`. Invoke as:

`/blueprint:design-doc`, `/blueprint:spec`, `/blueprint:plan`, `/blueprint:implement`, `/blueprint:tdd`, `/blueprint:refactor`, `/blueprint:review`, `/blueprint:browser-verify`, `/blueprint:branch`, `/blueprint:commit`

`browser-verify` requires Chrome DevTools MCP.

Keep shared repo guidance in `AGENTS.md`; keep Claude-specific adapter notes here.

## Claude-specific rules

1. **Think before coding.** State assumptions explicitly. If multiple interpretations exist, present them -- don't pick silently. If something is unclear, stop and ask before implementing.
2. **Simplicity first.** Minimum code that solves the problem. No speculative features, abstractions, or configurability beyond what was asked.
3. **Surgical changes.** Touch only what you must. Match existing style. Remove imports/variables/functions that your changes made unused. Every changed line should trace directly to the request.
4. **Goal-driven execution.** Transform tasks into verifiable goals before starting. For multi-step tasks, state a brief plan with success criteria per step. Loop until verified.
5. **CLI safety.** Never use `--verbose` in any CLI command -- verbose output can expose secrets and credentials in logs.
6. **300 LOC limit.** No source file should exceed 300 lines of code. Split by responsibility when a file crosses the limit.
