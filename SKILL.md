---
name: dev-workflow
description: Auto-discovers project configuration and manages task context using Spec-Driven Development (SDD). Use when the user explicitly references a ticket code, task name, or asks to start or continue work on a task. Loads ~/.dev-workflow/development.md silently if it exists.
---

# Development Workflow

## Auto-Load Development Environment

When this skill is loaded, **automatically check for and load** `~/.dev-workflow/development.md` if it exists. Follow [references/development-manager.md](references/development-manager.md) to load or create it.

- If the file exists: load it silently and use it as project context.
- If the file does not exist: follow [references/development-manager.md](references/development-manager.md) to auto-discover the settings and create it when a task is requested.

## Task Context (On-Demand Only)

Only load or create a task file (`~/.dev-workflow/{TICKET-CODE}.md`) when the user **explicitly requests** work on a task. Follow [references/task-manager.md](references/task-manager.md) when a task is requested.

Do **not** create ad-hoc tasks or assume a task context unless the user explicitly asks to start, continue, or work on something.

Once both development environment and task context are loaded, proceed with the user's request while considering both context sources for every decision.

When a task file is created or loaded, automatically proceed through the SDD phases up to Design (phase 3), then present the design to the user and ask for approval before continuing to Implement (phase 4).

## SDD Methodology

This skill follows **Spec-Driven Development (SDD)** — a software engineering methodology where structured, machine-readable specifications act as the primary source of truth before any code is written. In SDD, specifications become executable contracts that guide implementation and validate correctness. Every task flows through:

1. **Discovery** — Understand context and gather requirements.
2. **Specify** — Define the contract (requirements, acceptance criteria, boundaries).
3. **Design** — Create the technical plan and task breakdown.
4. **Implement** — Execute with validation checkpoints.
5. **Validate & Archive** — Verify against the spec and complete.

For a multi-agent breakdown of which agent should handle each phase, see [references/multi-agent-phases.md](references/multi-agent-phases.md).

Each time a phase is completed, update the task file with its current state and the timestamp of the last update. The task file should always reflect the current state of the task, including any new requirements or changes.

## Trigger Parsing

Only activate task loading when the user explicitly references a task:

- **Ticket codes**: `PROJ-123`, `TEAM_456`, `#789`, or any `{PREFIX}-{NUMBER}` pattern.
- **Task names**: If the user says *"continue X"*, *"work on X"*, or *"start X"*, use `X` as the task identifier.
- **Ad-hoc tasks**: Only create `adhoc-{timestamp}.md` if the user explicitly says something like *"start a new task"* or *"work on something new"* without a ticket code.

## Ad-Hoc Tasks

When the user explicitly requests a new task but no ticket code is provided:

1. Create `~/.dev-workflow/adhoc-{timestamp}.md` from the task profile template. Use an ISO-8601 compact timestamp with milliseconds, keeping the `T` date-time separator: `adhoc-YYYYMMDDTHHMMSSmmm.md` (for example, `adhoc-20260626T115825913.md`).
2. Set the **Context** section to:
   - **Code**: `adhoc-{timestamp}`
   - **URL**: (empty)
3. Populate **1. Specification** (Requirements, Acceptance Criteria, Boundaries) from the user's request.
4. Proceed through the SDD workflow: Design → (Implement only when explicitly asked) → Validate.

## Rules

- Reference the current task file for requirements and testing steps on every task.
- **All new requirements must be written to the task file (Phase 2: Specify) before implementation.**
- **Local project overrides**: If the current project's `AGENTS.md`, `CONTRIBUTING.md`, or `.github/PULL_REQUEST_TEMPLATE.md` contradicts a global dev-workflow rule, follow the **local** project rule and note the override in the task file.
- **Authority order**: Project-local `AGENTS.md` is the highest-authority source of project context. `development.md` supplements `AGENTS.md`; it does not replace it. When both files exist, prefer `AGENTS.md` for project-specific conventions and use `development.md` only for cross-project developer preferences and discovered links.
