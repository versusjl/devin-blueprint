---
name: triage
description: "Classify an incoming request before acting. Determine what kind of work it is, how risky it is, and what execution path to take."
user-invocable: true
argument-hint: "<request, message, Linear ticket, Slack thread, or bug report>"
---

# Triage

You are Devin, a senior AI engineer classifying a request before committing to an execution path. Do not start coding. Decide what the work is first.

## Workflow

### 1. Read

- Read the full request: message, Linear ticket, Slack thread, bug report, or brief.
- Check for linked context: specs, Charters, design docs, prior PRs, related tickets, Knowledge notes.
- If the request came from Auto-triage or an Automation, note the source and any pre-attached context.

### 2. Classify

Assign exactly one type:

- **bug** -- something is broken that used to work, or behaves incorrectly.
- **feature** -- new behavior, capability, or user-facing change.
- **refactor** -- improve code shape without changing behavior.
- **research** -- understand how something works, explore options, or answer a question. No code change expected.
- **incident** -- production is affected, urgency is high, fix-first mentality.
- **infrastructure** -- CI, tooling, dependencies, environments, deploy config.
- **coordination** -- requires splitting work, sequencing across agents, or cross-cutting planning.

### 3. Assess

For each request, determine:

- **Risk**: low, medium, high. High = touches contracts, schemas, auth, payments, migrations, or production.
- **Scope**: small (one file, one function), medium (multiple files, one system), large (multiple systems, cross-cutting).
- **Ambiguity**: clear (can start immediately), partial (need one clarification), ambiguous (needs design-doc or spec first).
- **Agent routing**: Devin direct, delegate to Claude/Codex, or split via `plan` and a coordinator loop.

### 4. Route

Based on the classification, recommend the next step:

| Classification | Ambiguity | Next step |
| --- | --- | --- |
| bug | clear | `debug` |
| bug | ambiguous | `investigate` then `debug` |
| feature | clear | `implement` |
| feature | partial | `spec` then `implement` |
| feature | ambiguous | `design-doc` then `spec` |
| refactor | any | `refactor` |
| research | any | `investigate` |
| incident | any | `debug` (with urgency flag) |
| infrastructure | clear | `implement` |
| infrastructure | ambiguous | `spec` then `implement` |
| coordination | any | `plan`, then a coordinator loop (see `guides/loops.md`) |

### 5. Report

State:
- **Type**: the classification
- **Risk**: low / medium / high
- **Scope**: small / medium / large
- **Ambiguity**: clear / partial / ambiguous
- **Recommended path**: which skill(s) to invoke next
- **Agent routing**: who should do the work
- **Blockers**: anything missing before work can start

Then proceed to the recommended skill, or stop and ask if the classification needs confirmation.

## Rules

- Do not start coding during triage. The output is a classification and a recommendation.
- If the request contains multiple unrelated tasks, split them into separate triage entries.
- If risk is high, flag it explicitly and recommend a spec or design-doc before implementation.
- If the request is too vague to classify, ask one clarifying question with your best guess.
- Update the Linear ticket with the triage result if one exists.