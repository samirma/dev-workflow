# dev-workflow

An AI-agent skill for ticket-backed and ad-hoc development tasks using **Spec-Driven Development (SDD)**.

## Behavior

For non-ticket work, the skill starts a task only when the user explicitly creates, starts, or begins a `task` or `devtask`; those words are equivalent. At that exact moment it captures the raw requirements, establishes one active task context, and runs Discovery, Specification, and Design. Implementation waits for a later explicit instruction.

Ticket work also activates from existing work intent such as `Work on PROJ-123`, `continue PROJ-123`, or a direct ticket reference used as the requested task. It does not require create/start/begin wording.

Ticket tokens take precedence over descriptive requirements. A ticket task uses its exact ID and `~/.dev-workflow/{TASK-ID}.md`; file existence decides routing, regardless of whether the prompt says *create* or *load*:

- Existing ticket file → load it and resume without re-running completed phases.
- Missing ticket file → create it and begin the SDD workflow.

An ad-hoc task derives a concise descriptive name from the requested work and keeps all bookkeeping exclusively in current runtime memory. It has no generated ID, task file, alternate record, cache, database entry, or persistent-memory entry. Source code and tests are ordinary project files, and `~/.dev-workflow/development.md` remains the allowed shared project configuration file.

The ad-hoc lifecycle remains in the Create controller for questions, changed requirements, implementation, validation, and completion. If runtime context is lost, the agent asks for requirements and current progress; it never searches for a file.

## Prompt-routing examples

The following user prompt families are intentionally supported. Prompts are shown verbatim, including their original spelling.

| Prompt | Result |
|---|---|
| `create a new devtask for the ticket XPTO-1234` | Use exact ID `XPTO-1234`; load `~/.dev-workflow/XPTO-1234.md` if it exists, otherwise create it. |
| `load the devtask XPTO-1234` | Same existence-based ticket routing. |
| `create a new task for the ticket XPTO-1234` | Same existence-based ticket routing; `task` and `devtask` are equivalent. |
| `create devtask to implemant action butoon to do such and such` | Start a runtime-only ad-hoc task with a descriptive name based on the action-button work; retain the entire prompt as raw requirements. |
| `create task to chancge the behovior of the method valdateAccoiunt to use the new lib xyn` | Start a runtime-only ad-hoc task named from the `valdateAccoiunt`/`xyn` change; create no bookkeeping file. |

If a request says only “create a task” without actionable requirements or a ticket token, the agent asks for requirements before initializing anything. Requirements by themselves and general design discussion do not start a task.

## SDD phases

| Phase | Purpose |
|---|---|
| **1. Discovery** | Collect project and task-source facts. |
| **2. Specification** | Define requirements, acceptance criteria, boundaries, and product scenarios. |
| **3. Design** | Plan the technical change, Progress checklist, and technical tests. |
| **4. Implementation** | Execute the plan after a separate explicit implementation request. |
| **5. Validation** | Run every required check and set `Done` only after all gates succeed. |

Every phase operates on the same active task context and generic Task Source. Storage is isolated behind a ticket-file or runtime-memory adapter, so the SDD phases do not branch on task format.

## Installation

Clone the repository into the skills directory used by your AI-agent CLI. For a CLI that reads `~/.agents/skills/`:

```bash
git clone git@github.com:samirma/dev-workflow.git ~/.agents/skills/dev-workflow
```

Restart the CLI or open a new session, then use one of the prompt families above.

## Instruction files

| File | Responsibility |
|---|---|
| `SKILL.md` | Trigger parsing and top-level routing. |
| `references/task-manager.md` | Active context, adapters, phases, gates, and shared rules. |
| `references/task-create.md` | Common task creation and the complete runtime-memory lifecycle. |
| `references/task-load.md` | Restore an existing persisted ticket task. |
| `references/task-work.md` | Continue or refresh a persisted ticket task. |
| `references/task-profile-template.md` | Persisted ticket-task file shape. |
| `references/development-manager.md` | Discover and create shared project configuration. |
| `references/development-environment-template.md` | Shared project configuration template. |
| `references/phases/` | One focused instruction file per SDD phase. |

## Requirements

- An AI-agent CLI that loads local skills.
- A ticket manager integration is optional. When ticket evidence cannot be fetched, the workflow asks the user for the relevant content rather than guessing.

## License

MIT
