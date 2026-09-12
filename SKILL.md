---
name: dev-workflow
description: Auto-discovers project configuration and manages ticket-based or in-memory development tasks using Spec-Driven Development (SDD). Use when the user references a ticket, asks to create, load, continue, or implement a task or devtask, or starts a new task with raw requirements.
---

# Development Workflow

## Project environment

At activation, silently load `~/.dev-workflow/development.md` when it exists. When a task is actually initiated and the file is missing, follow [`references/development-manager.md`](references/development-manager.md) to create it. Do not create project configuration for discussion that has not initiated a task.

## Trigger and identity

Treat **task** and **devtask** as equivalent words.

A ticket task activates when the user supplies a ticket token with task or work intent, including requests to create, load, continue, or work on it. A direct ticket reference used as the requested task also activates this workflow. Preserve this existing ticket behavior.

A non-ticket ad-hoc task begins at the exact point where the user explicitly asks to create, start, or begin a task or devtask. Do not initialize an ad-hoc task from requirements alone, a general question, or discussion of possible work.

Resolve identity in this order:

1. **Ticket token takes precedence.** If the initiating or loading request contains a ticket token such as `PROJ-123`, `TEAM_456`, or `#789`, preserve the token exactly and use the ticket workflow. Test `~/.dev-workflow/{TASK-ID}.md`: an existing file loads; a missing file creates. The words *create* and *load* do not override this existence check.
2. **Raw requirements create an ad-hoc task.** If no ticket token is present, immediately retain the user's complete initiating requirements in the runtime active-task context before summarising or analysing them. Derive a concise descriptive task name from the work itself. The name is only a runtime label: do not generate an ID, path, task file, alternate record, or persistent-memory entry.

If a non-ticket initiation does not contain actionable requirements, ask for them before initializing the task. If an ad-hoc continuation has lost its runtime context, ask for the requirements and current progress; never search for a saved record.

## SDD model

Every active task follows Discovery, Specification, Design, Implementation, and Validation in order. The active-task model, storage adapters, continuation rules, phase index, gates, and shared SDD rules are defined in [`references/task-manager.md`](references/task-manager.md).

## Routing

| Situation | Route |
|---|---|
| Ticket token whose exact task file exists | [`references/task-load.md`](references/task-load.md) |
| Ticket token whose exact task file is missing | [`references/task-create.md`](references/task-create.md) |
| New non-ticket task with raw requirements | *Create*; remain in that controller for its entire runtime lifecycle |
| Follow-up for the active ad-hoc task | *Create* again; resume the same runtime context |
| Follow-up for an active persisted ticket task | [`references/task-work.md`](references/task-work.md) |

An ad-hoc task never enters *Load* or *Work*. A persisted ticket task never substitutes a descriptive name for its exact ticket ID.
