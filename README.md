# dev-workflow

A generic AI agent skill that auto-discovers project configuration and manages task context using **Spec-Driven Development (SDD)**.

## What it does

This skill gives the agent full development context before it writes or edits any code. It follows a structured **Spec-Driven Development (SDD)** workflow where specifications are written *before* implementation.

It maintains two types of files in `~/.dev-workflow/`:

1. **`development.md`** — Global project environment (developer info, ticket manager URL, local codebases, workflow rules). Acts as the **project constitution**.
2. **`{TICKET-CODE}.md`** — Per-task spec file structured into SDD phases: Specification → Design → Implementation → Validation.

When you start a task, mention a ticket code, or ask for implementation work, the skill:
- Auto-detects or creates the project environment file.
- Loads or creates a task file for the current ticket.
- Guides the agent through the SDD workflow: **Discover → Specify → Design → Implement → Validate**.
- Ensures requirements and acceptance criteria are captured before any code is written.
- Keeps ticket and PR status synced as work progresses.

## SDD Workflow

Every task flows through five phases:

| Phase | Purpose |
|-------|---------|
| **1. Discovery** | Load project context, fetch ticket details, understand scope. |
| **2. Specify** | Write requirements, acceptance criteria, and boundaries. |
| **3. Design** | Create technical plan, file structure, and task breakdown. |
| **4. Implement** | Execute tasks with progress tracking and TDD where appropriate. |
| **5. Validate & Archive** | Verify against the spec, review, and complete. |

## Installation

### 1. Clone the skill

```bash
git clone git@github.com:samirma/dev-workflow.git ~/.agents/skills/dev-workflow
```

> Your AI agent CLI looks for skills under `~/.agents/skills/` by default. If your CLI is configured to use a different path, clone there instead.

### 2. Verify it loads

Restart your AI agent CLI or start a new session. The skill should appear in the available skills list.

### 3. Start using it

Begin any development request. The skill will trigger automatically and either load your existing `~/.dev-workflow/development.md` or guide you through creating one.

## Quick start

```
Work on PROJ-123
```

The agent will:
1. Discover your project settings.
2. Create `~/.dev-workflow/PROJ-123.md` from the task profile template.
3. Pull ticket details (title, acceptance criteria, etc.) from your ticket manager.
4. Write the specification (requirements, boundaries) before writing any code.
5. Create a technical design and task breakdown.
6. Implement with validation checkpoints.

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill manifest and trigger rules |
| `references/development-manager.md` | Instructions for managing `development.md` (project constitution) |
| `references/task-manager.md` | Instructions for managing per-task spec files |
| `references/task-profile-template.md` | SDD-aligned task spec template |

## Requirements

- A ticket manager URL (Jira, Linear, etc.) is optional but recommended

## License

MIT
