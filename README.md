# dev-workflow

A Kimi Code CLI skill that auto-discovers project configuration and manages task context before any code change.

## What it does

This skill gives the agent full development context before it writes or edits any code. It works by maintaining two types of files in `~/.dev-workflow/`:

1. **`development.md`** — Global project environment (developer info, ticket manager URL, local codebases, workflow rules).
2. **`{TICKET-CODE}.md`** — Per-task context (acceptance criteria, technical requirements, testing steps, PR status).

When you start a task, mention a ticket code, or ask for implementation work, the skill:
- Auto-detects or creates the project environment file.
- Loads or creates a task file for the current ticket.
- Ensures requirements are captured before any code is written.
- Keeps ticket and PR status synced as work progresses.

## Installation

### 1. Clone the skill

```bash
git clone git@github.com:samirma/dev-workflow.git ~/.agents/skills/dev-workflow
```

> Kimi Code CLI looks for skills under `~/.agents/skills/` by default. If your CLI is configured to use a different path, clone there instead.

### 2. Verify it loads

Restart Kimi Code CLI or start a new session. The skill should appear in the available skills list.

### 3. Start using it

Begin any development request. The skill will trigger automatically and either load your existing `~/.dev-workflow/development.md` or guide you through creating one.

## Quick start

```
Work on PROJ-123
```

The agent will:
1. Discover your project settings.
2. Create `~/.dev-workflow/PROJ-123.md` from the task template.
3. Ask if you want to pull ticket details (title, acceptance criteria, etc.) from your ticket manager.
4. Capture everything before writing any code.

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill manifest and trigger rules |
| `references/development-manager.md` | Instructions for managing `development.md` |
| `references/task-manager.md` | Instructions for managing per-task files |

## Requirements

- [Kimi Code CLI](https://github.com/samirma/kimi-cli) (or any compatible CLI that supports the skill system)
- A ticket manager URL (Jira, Linear, etc.) is optional but recommended

## License

MIT
