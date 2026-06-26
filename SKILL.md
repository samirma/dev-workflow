---
name: dev-workflow
description: Development workflow that auto-discovers project configuration and manages task context using Spec-Driven Development (SDD). Use when the user starts a new task, mentions a ticket code, continues working on an existing task, updates requirements, asks to check ticket or PR status, or requests any implementation work. Triggers on ticket IDs, task names, Jira references, or any development request.
---

# Development Workflow

## Auto-Load Development Environment

When this skill is loaded, **automatically check for and load** `~/.dev-workflow/development.md` if it exists. Follow [references/development-manager.md](references/development-manager.md) to load or create it.

- If the file exists: load it silently and use it as project context.
- If the file does not exist: do **not** prompt the user to create it unless they explicitly request a task that requires it.

## Task Context (On-Demand Only)

Only load or create a task file (`~/.dev-workflow/{TICKET}.md`) when the user **explicitly requests** work on a task. Follow [references/task-manager.md](references/task-manager.md) when a task is requested.

Do **not** create ad-hoc tasks or assume a task context unless the user explicitly asks to start, continue, or work on something.

Once both development environment and task context are loaded, proceed with the user's request while considering both context sources for every decision.

WHen the a task is being created it should follow automatically the SDD phases until the phase 3 then appresent the user with the design and ask for approval to continue to phase 4.

## SDD Methodology

This skill follows **Spec-Driven Development (SDD)** — a software engineering methodology where structured, machine-readable specifications act as the primary source of truth before any code is written. In SDD, specifications become executable contracts that guide implementation and validate correctness. Every task flows through:

1. **Discovery** — Understand context and gather requirements.
2. **Specify** — Define the contract (requirements, acceptance criteria, boundaries).
3. **Design** — Create the technical plan and task breakdown.
4. **Implement** — Execute with validation checkpoints.
5. **Validate & Archive** — Verify against the spec and complete.

For a multi-agent breakdown of which agent should handle each phase, see [references/multi-agent-phases.md](references/multi-agent-phases.md).

Each time that a phase is completed, the skill should update the task file with is current state and the timestamp of the last update. The task file should always reflect the current state of the task, including any new requirements or changes.

## Trigger Parsing

Only activate task loading when the user explicitly references a task:

- **Ticket codes**: `PROJ-123`, `TEAM_456`, `#789`, or any `{PREFIX}-{NUMBER}` pattern.
- **Task names**: If the user says *"continue X"*, *"work on X"*, or *"start X"*, use `X` as the task identifier.
- **Ad-hoc tasks**: Only create `adhoc-{timestamp}.md` if the user explicitly says something like *"start a new task"* or *"work on something new"* without a ticket code.

## Ad-Hoc Tasks

When the user explicitly requests a new task but no ticket code is provided:

1. Create `~/.dev-workflow/adhoc-{timestamp}.md` from the task profile template. Use an ISO-8601 compact timestamp with milliseconds and no separators: `adhoc-YYYYMMDDTHHMMSSmmm.md` (for example, `adhoc-20260626T115825913.md`).
2. Set the **Context** section to:
   - **Code**: `adhoc-{timestamp}`
   - **URL**: (empty)
3. Populate **1. Specification** (Requirements, Acceptance Criteria, Boundaries) from the user's request.
4. Proceed through the SDD workflow: Design → (Implement only when explicitly asked) → Validate.

## Rules

- Auto-load `development.md` silently if it exists;
- Only load or create task context when the user explicitly requests it.
- Reference the current task file for requirements and testing steps on every task.
- **All new requirements must be written to the task file (Phase 2: Specify) before implementation.**
- Do not run `git commit`, `git push`, or `git rebase` unless explicitly instructed.
- **Local project overrides**: If the current project's `AGENTS.md`, `CONTRIBUTING.md`, or `.github/PULL_REQUEST_TEMPLATE.md` contradicts a global dev-workflow rule, follow the **local** project rule and note the override in the task file.
- **Authority order**: Project-local `AGENTS.md` is the highest-authority source of project context. `development.md` supplements `AGENTS.md`; it does not replace it. When both files exist, prefer `AGENTS.md` for project-specific conventions and use `development.md` only for cross-project developer preferences and discovered links.
