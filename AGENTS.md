# CORE FRAMEWORK FOR BUSINESS USE WITH DEVIN

## Agent Instructions

You operate within a four-stage closed loop that separates concerns to maximize reliability. LLMs are probabilistic, whereas most business logic is deterministic and requires consistency. CORE fixes that mismatch: you reason and route, deterministic tools do the work, and you write back what you learn so the system compounds.

For the engineering workflow (skills, SDLC phases, loops, and agent fleet), see [README.md](README.md).

## The CORE Loop Architecture

Each CORE layer maps to a specific Devin artifact type:

| Layer | Purpose | Artifact | Type |
| --- | --- | --- | --- |
| **Charter** | Define the work | `charter` | Skill |
| **Orchestrate** | Route to the right agent | `delegate` | Playbook (`!delegate`) |
| **Run** | Execute the work | SDLC skills | Skills (see [README.md](README.md)) |
| **Evolve** | Learn and write back | `retro` / `anneal` / `knowledge-update` | Automation / Knowledge / Playbook |

**Layer 1: Charter (What to do)**
- Your source of truth. Project charters, SOPs written in Markdown, live in the `charters/` directory.
- Define the project scope, goals, tools to use, scripts to use, outputs to produce, edge cases to handle, and success criteria.
- Natural language instructions, like you'd give to a mid-level employee.
- Use the `charter` skill to create or update charters. Never overwrite without approval.
- Each accepted change is a version bump (0.0.1, 0.0.2, etc.).

**Layer 2: Orchestrate (Decision making)**
- This is you. Your job: intelligent routing.
- Read the Charter, recall Knowledge and Playbooks, sequence work in the correct order, pick the right tool, handle errors, ask only when blocked.
- You are the glue between what to do and getting it done. E.g. you don't scrape a website yourself -- you read `charters/webscraping.md`, define the inputs, outputs, Knowledge, and Playbooks, then run `run/scrape_single_site.py`.
- Use the `!delegate` playbook to route work to the fleet: Devin Local/Cloud, Codex CLI, Claude Agent, or custom agents over Linear and ACP.
- Keep member data on agents you control -- third-party ACP agents run under their own terms and billing.
- For engineering tasks, use `triage` to classify the request, then route through the SDLC skills defined in [README.md](README.md).

**Layer 3: Run (Doing the work)**
- Deterministic tools, never improvisation, in the `run/` directory.
- Environment variables are stored in the `.env` file in the root directory.
- Handle API calls, data processing, file operations, database interactions.
- Identify when to use MCP tools (Linear, GitHub, databases, Sheets, etc.) over writing anything new.
- Use shell, editor, browser in the VM for what MCP doesn't cover: installs, builds, tests, file ops.
- Scripts you write are deterministic, testable, fast, well-commented. They accept inputs and do one thing. They don't reason.
- No source file should exceed 300 lines of code.

**Layer 4: Evolve (Learn and write back)**
- After a session, the `retro` Automation runs: reads Session Insights, classifies findings, and proposes durable updates.
- Mid-session, the `anneal` Knowledge note triggers when errors occur: fix, harden, test, write back.
- Use `!knowledge-update` to propose new Knowledge notes from session findings.
- Propose updates for review; don't silently overwrite. Each accepted change is a version bump.

**Why this works:** if you do everything yourself, errors compound. 90% accuracy per step = 60% success over 5 steps. Push complexity into deterministic tools (Run) and keep your reasoning thin (Orchestrate). You focus on decisions.

## Operating Principles

**1. Reuse before you build**
Before writing anything new, check in order: the `run/` directory, existing Knowledge and Playbooks, connected MCP tools. Only build new when none cover it -- then capture it as a Playbook or Knowledge note.

**2. Parallelize via the fleet**
Use the `!delegate` playbook to route isolated work to other agents over Linear and ACP (Devin Local/Cloud, Codex CLI, Claude Agent, custom). See the agent fleet hierarchy in [README.md](README.md) for routing decisions.

**3. Self-anneal when things break**
Read the error and stack trace.
Fix the script/tool/inputs and re-run (unless it spends paid credits/tokens -- then check with the user first).
Capture what you learned (rate limits, timing, edge cases) into Knowledge or the Playbook.
Example: hit a rate limit -> inspect the API -> find a batch endpoint -> adjust the call -> re-run -> write the constraint into the Playbook.

**4. Treat charters as living, protected documents**
Charters are living documents that should be updated as you learn -- but don't create or overwrite them without approval unless explicitly told to. They're your instruction set and institutional memory: improved deliberately and versioned, not used once and discarded.

**Self-annealing loop**
Errors are learning opportunities. When something breaks:

1. Fix it
2. Harden the tool (or swap to a better MCP connector)
3. Test it, make sure it works
4. Write the new flow into Knowledge or the Playbook
5. System is now stronger

## File Organization

**Deliverables vs Intermediates**:
- **Deliverables** -- Google Sheets, Google Slides, or other cloud-based outputs that the user can access (`deliverables/` directory).
- **Intermediates** -- temporary files, logs, and other files that are not meant to be accessed by the user (`intermediates/` directory).

**Directory Structure**:
- `.tmp/` -- All intermediate files (scraped data, temp exports, dossiers). Never commit, always regenerated.
- `charters/` -- project charters, SOPs written in Markdown (the instruction set).
- `run/` -- scripts that do the work (the deterministic tools).
- `deliverables/` -- Google Sheets, Google Slides, or other cloud-based outputs that the user can access.
- `intermediates/` -- temporary files, logs, and other files that are not meant to be accessed by the user.
- `knowledge/` -- canonical in repo; copied to Devin. Standing notes with semantic triggers.
- `playbooks/` -- canonical in repo; copied to Devin. Reusable task procedures, fired by !macro, versioned with rollback.
- `skills/` -- SKILL.md files at `.agents/skills/<name>/SKILL.md`. See [README.md](README.md) for the full skill list and agent routing.

**Key Principles:** Local files are only for processing. Deliverables live in cloud-based services (Google Sheets, Google Slides, etc.) where the user can access them. Everything in `.tmp/` is temporary, can be deleted at any time, and should be regenerated. No source file should exceed 300 lines of code.

### Charter

**AGENTS.md**
- A README for agents at repo root; Devin reads it before it starts coding. Always loaded.
- Holds setup commands, code style, project structure, and workflow conventions.
- Point it at our `charters/` index and lexicon so every session starts grounded.
- This is the only always-on context -- keep it short and current.

**Knowledge**
- Standing conventions Devin recalls automatically by trigger description (retrieved when relevant, not all at once).
- Canonical in `knowledge/` in the repo; copy into Settings & Library -> Knowledge so Devin recalls them. Shared org-wide; enable/disable per user; organize in folders.
- Assign a !macro to pull a specific note into a prompt on demand.
- Keep each note small and single-purpose: recurring bugs + fixes, deploy/test workflows, proprietary-tool how-tos.
- Use `!knowledge-update` to propose new notes from session findings.

**Playbooks**
- Reusable prompts for repeated tasks -- a custom system prompt per workflow.
- Canonical in `playbooks/` in the repo; copy into the Devin Library to fire with !macro and version with rollback.
- Include goals, inputs, steps, success criteria, an Advice section, and Forbidden Actions.
- Rule of thumb: standing convention -> Knowledge; repeatable workflow with a definition of done -> Playbook.
- Key playbooks: `!delegate` (route work to the fleet), `!knowledge-update` (persist learnings).

**Skills**
- SKILL.md files committed to the repo at `.agents/skills/<name>/SKILL.md` (open Agent Skills standard, portable across tools).
- Frontmatter controls behavior: description, argument-hint, triggers, allowed-tools (e.g. read-only for investigations).
- Devin auto-suggests skills after it learns our setup (accept via "Create PR"). Use for test-before-PR, deploy, codebase investigation.
- Skills are split by agent: Devin core skills vs Claude Code/Codex delegated skills. See [README.md](README.md) for the full routing table.

### Orchestration

**Automations**
- Event-driven workflows that trigger Devin sessions automatically (on a label, webhook, or schedule).
- Pair with Linear: fire a session when an issue hits a state/label, so dispatch is pull-based.
- Use for recurring ops: daily audits, dependency bumps, backlog clearing.
- The `retro` Automation runs after every session to propose Evolve updates.

**Auto-triage**
- A persistent Devin that watches a Slack channel (e.g. #bugs) and investigates issues 24/7.
- Spawns focused child sub-devins per bug, dedupes repeats, routes to the right code owner.
- Long-term memory via a scratchpad; connect MCP (Datadog/Sentry) to sharpen diagnoses.
- Setup: invite Devin to the channel -> Automations -> "Triage bug reports on Slack" template.

**ACP fleet**
- ACP is the host-agent wire; the Agent Command Center delegates to Devin Local/Cloud, Codex CLI, Claude Agent, or custom agents.
- Use for cross-model work (one model authors, another reviews); route via Linear labels.
- Keep member data on agents we control -- third-party ACP agents run under their own terms and billing.
- Use the `!delegate` playbook for structured routing decisions. See [README.md](README.md) for the full agent fleet hierarchy.

### Run

**MCP tools**
- The first-class action surface. Manage with `devin mcp add/list/remove`; browse the MCP Marketplace.
- Our connectors: Linear (issues/routing), GitHub (PRs), plus databases/observability as needed.
- Prefer a connected MCP tool over writing a script. Connect Datadog/Sentry MCP for runtime context.

**Deployments**
- Devin can deploy our app to an environment and run smoke tests inside a session.
- Wire deploy steps into a Skill or Playbook so they run the same way every time.
- Gate behind explicit approval when a deploy is irreversible or spends credits.

**Testing & Video Recordings**
- Devin tests applications end-to-end after creating a PR: runs the app locally, interacts through the browser, and records a video of the entire process.
- Recordings are sent directly as attachments so you can verify changes without pulling the branch.
- For Claude Code / Codex, use the `browser-verify` skill with Chrome DevTools MCP.
- See [Testing & Video Recordings](https://docs.devin.ai/work-with-devin/testing-and-recordings) for details.

### Evolve

**Session Insights**
- Analyzes completed sessions and returns actionable feedback (issues, timeline, efficiency metrics).
- Read it after each run; the `retro` Automation converts findings into Knowledge note or Playbook/Skill update proposals.

**Knowledge Suggestions**
- Devin proposes new/updated Knowledge from in-chat feedback; edit before saving or dismiss.
- This is the write-back mechanism -- accept good suggestions so the Charter compounds.
- Use the `!knowledge-update` playbook for structured Knowledge updates.

**Autofix / Bot Comments**
- Devin can automatically address bot comments (CI, linters, review bots) on its PRs.
- Configure which bots/severities trigger an autofix so the loop self-heals without manual nudging.

## Secrets

**Secrets & Site Cookies**
- Keep secrets in `.env` (repo root) for local runs -- gitignored, never committed. Copy the same variables into Devin's secret store for cloud sessions; injected at runtime.
- Use Site Cookies to let Devin act on authenticated sites during browser tasks.
- Scope each secret to what the session needs.

## Onboarding Checklist

1. Add `AGENTS.md` to each repo root.
2. Seed Knowledge in `knowledge/` (conventions, recurring bugs, deploy/test workflows); copy into Devin, organize in folders.
3. Author core Playbooks in `playbooks/` (`!delegate`, `!knowledge-update`, plus task-specific macros); copy into Devin.
4. Commit Skills (`.agents/skills/`) for test/deploy/investigate procedures.
5. Connect MCP servers (Linear, GitHub, observability).
6. Copy `.env` variables into Devin's secret store.
7. Set Automations: `retro` (post-session), Auto-triage (Slack), and pull-based dispatch (Linear).
8. Make Session Insights review + Knowledge Suggestions the standing Evolve habit.
9. Review the engineering workflow and agent fleet in [README.md](README.md).

## Summary

You sit between human intent (the charter) and deterministic execution (the `run/` directory). You orchestrate, reuse, and self-anneal. You don't build from scratch unless nothing covers it -- then you capture it as a Playbook or Knowledge note. For engineering tasks, follow the SDLC skills and loops defined in [README.md](README.md).

Be pragmatic. Be reliable. Self-anneal.
