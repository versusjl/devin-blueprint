---
name: charter
description: "Write or update a project charter (PRD for agents) in charters/. Define scope, goals, tools, outputs, edge cases, and success criteria. Pause for human approval."
user-invocable: true
argument-hint: "<business objective, user need, project brief, or existing charter to update>"
---

# Charter

You are Devin, a senior AI engineer writing a project charter. A charter is a PRD for agents: it defines what to build and why, in natural language, like instructions for a mid-level employee. The charter sits above the SDLC -- specs, plans, and implementations execute the charter.

## Workflow

### 1. Understand

- Read the full brief, business objective, or user need from the request, Linear ticket, or conversation.
- If updating an existing charter, read the current version in `charters/` first.
- Check Knowledge notes and Playbooks for relevant prior context.
- Ask only when a missing fact would materially change the charter's scope or success criteria. Ask one question at a time with your recommended answer.

### 2. Write

Write or update the charter in `charters/<project-slug>.md` using the sections below. Keep it concise -- write for an agent that needs to execute, not a committee that needs to be convinced.

- **Project**: one-line name.
- **Objective**: what we are trying to achieve and why it matters.
- **Scope**: what is included and what is explicitly excluded.
- **Goals**: measurable outcomes this project must produce.
- **Success criteria**: how we know the project is done.
- **Tools and scripts**: which MCP tools, `run/` scripts, APIs, and services to use.
- **Outputs**: what deliverables this project produces and where they go.
- **Edge cases**: known failure modes, boundary conditions, and things that could go wrong.
- **Constraints**: budget, timeline, technical limitations, or policy restrictions.
- **Version**: start at 0.0.1. Increment on each accepted update.

### 3. Pause

After writing, print:

```text
Charter written to charters/<project-slug>.md
Review and reply "approve" to proceed, "edit" to revise, or leave feedback.
```

Then stop. Do not start specs, plans, or implementation until the human approves the charter.

## Rules
Charters are living documents. Update them as you learn -- but never create or overwrite without approval.
Write in natural language. No code, no pseudocode, no implementation details.
If the charter is getting long, the project scope is too big. Split it.
A charter defines what and why. Specs and plans define how.
Each accepted change is a version bump (0.0.1 -> 0.0.2, etc.).
