---
name: design-doc
description: "Write a lightweight technical design document for ambiguous or consequential architecture decisions before implementation."
user-invocable: true
argument-hint: "<system, feature, architecture question, repo path, or design brief>"
---

# Design Doc

Write a lightweight architecture design doc for a decision that deserves thinking before code. Use this when the problem has real ambiguity, tradeoffs, cross-cutting concerns, or future maintenance risk. If the solution is obvious and the doc would only become an implementation manual, say so and recommend `spec` or `implement` instead.

## Workflow

### 1. Ground

- Treat the full argument as the design brief unless the user names a feature or path.
- Derive a kebab-case slug if no name is given.
- Read referenced docs, code, specs, plans, Linear tickets, knowledge notes, playbooks, and architecture notes so the design fits the system as it exists.
- Use Devin's codebase search, MCP integrations, and session context to understand the current state before proposing changes.
- Identify stakeholders, goals, non-goals, constraints, cross-cutting concerns, and decisions that need human review.
- Ask only when a missing fact would materially change the design. Ask one question at a time with your recommended answer.

### 2. Write

Write `docs/<design-slug>/design.md` using the sections below. Keep docs short: prefer 1-3 pages unless the architecture genuinely needs more. Compress context aggressively -- every word competes for attention.

- **Status**: Draft, In Review, Approved, or Superseded.
- **Summary**: the problem and recommended direction in a few sentences.
- **Context and Scope**: objective background, current system state, and what this doc covers.
- **Goals**: outcomes this design must achieve.
- **Non-Goals**: reasonable adjacent outcomes this design deliberately excludes.
- **Constraints**: technical, product, migration, compatibility, operational, cost, or time constraints.
- **Proposed Design**: the chosen architecture, starting high-level before details.
- **Architecture Views** *(when useful)*: system context, data flow, runtime, deployment, or component diagrams. Mermaid diagrams are preferred when they clarify relationships.
- **Interfaces and Data** *(when relevant)*: API, event, schema, storage, or contract changes that affect the design.
- **Alternatives Considered**: plausible options and why they lose under the stated goals and constraints.
- **Tradeoffs**: what the chosen design gains, gives up, risks, or postpones.
- **Cross-Cutting Concerns**: security, privacy, observability, reliability, performance, cost, and operations where relevant.
- **Rollout and Migration** *(when relevant)*: how to ship, migrate, verify, and back out safely.
- **Open Questions**: decisions or facts still unresolved.
- **Decision**: the recommendation, assumptions, and who needs to review or approve it.

### 3. Pause

Stop after writing. Do not continue into `spec`, `plan`, or implementation until the human confirms the design.

If the design affects a Linear ticket, update the ticket with a link to the design doc and move its status to reflect the design review phase.

If the design affects a Charter, note the dependency but do not update the Charter until the design is approved.

## When Devin writes a design doc directly

Devin should write or draft a design doc itself when:

- the architecture is ambiguous and the user has not provided a direction
- the work touches multiple systems, schemas, or contracts
- there is a meaningful tradeoff between competing approaches
- a future maintainer will need to understand why this path was chosen
- the CORE Orchestrate layer identifies the task as requiring a design pass before execution

## When Devin delegates the design doc

Devin should delegate the design doc to another agent (Claude, Codex) when:

- the scope is narrow and well-bounded
- there is a clear template or prior design doc to follow
- the decision does not require deep codebase context that only Devin has

Devin still reviews the delegated design doc before it moves to the next phase.

## Rules

- The document exists to make reasoning reviewable, not to document every implementation step.
- Focus on tradeoffs, alternatives, and constraints that code will not explain later.
- Use diagrams only when they clarify relationships, flows, or deployment.
- Do not copy full schemas, APIs, or code unless the exact shape is central to the decision.
- Write for a future maintainer who needs to understand why this design was chosen.
- Propose the design doc as a deliverable; do not silently overwrite existing design docs without approval.
- If the design reveals that the original request should change, stop and update the user before continuing.