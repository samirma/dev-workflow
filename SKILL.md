---
name: dev-workflow
description: Development workflow that auto-discovers project configuration and manages task context using Spec-Driven Development (SDD). Use when the user starts a new task, mentions a ticket code, continues working on an existing task, updates requirements, asks to check ticket or PR status, or requests any implementation work. Triggers on ticket IDs, task names, Jira references, or any development request.
---

# Development Workflow

Give the agent full development context before any code change. Context comes from two sources:

1. **Development environment** (`~/.dev-workflow/development.md`) — follow [references/development-manager.md](references/development-manager.md) to load or create it.
2. **Current task** (`~/.dev-workflow/{TICKET}.md`) — follow [references/task-manager.md](references/task-manager.md) to load or create it.

Once loaded, proceed with the user's request while considering both context sources for every decision.

## SDD Methodology

This skill follows **Spec-Driven Development (SDD)** — a software engineering methodology where structured, machine-readable specifications act as the primary source of truth before any code is written. In SDD, specifications become executable contracts that guide implementation and validate correctness. Every task flows through:

1. **Discovery** — Understand context and gather requirements.
2. **Specify** — Define the contract (requirements, acceptance criteria, boundaries).
3. **Design** — Create the technical plan and task breakdown.
4. **Implement** — Execute with validation checkpoints.
5. **Validate & Archive** — Verify against the spec and complete.

## Trigger Parsing

Detect the ticket code or task identifier from the user's first message:

- **Ticket codes**: `PROJ-123`, `TEAM_456`, `#789`, or any `{PREFIX}-{NUMBER}` pattern.
- **Task names**: If the user says *"continue X"* or *"work on X"*, use `X` as the task identifier.
- **Ad-hoc tasks**: If no ticket code or task name is found, create an ad-hoc task file named `adhoc-{timestamp}.md`.

## Ad-Hoc Tasks

When no ticket code is detected:

1. Create `~/.dev-workflow/adhoc-{timestamp}.md` from the task profile template.
2. Set the **Context** section to:
   - **Code**: `adhoc-{timestamp}`
   - **URL**: (empty)
3. Populate **1. Specification** (Requirements, Acceptance Criteria, Boundaries) from the user's request.
4. Proceed through the SDD workflow: Design → Implement → Validate.

## Rules

- Never act before both config and task context are loaded.
- Reference `development.md` for codebase scope and workflow rules on every task.
- Reference the current task file for requirements and testing steps on every task.
- **All new requirements must be written to the task file (Phase 2: Specify) before implementation.**
- Do not run `git commit`, `git push`, or `git rebase` unless explicitly instructed.
- **Local project overrides**: If the current project's `AGENTS.md`, `CONTRIBUTING.md`, or `.github/PULL_REQUEST_TEMPLATE.md` contradicts a global dev-workflow rule, follow the **local** project rule and note the override in the task file.
