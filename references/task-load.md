# Loading a Persisted Ticket Task

Use this controller only when the exact ticket file already exists. The presence of the file, not whether the user said *create* or *load*, selects this route.

Apply the active-context and completed-phase rules from [`task-manager.md`](task-manager.md).

## Procedure

1. Read the complete ticket file into the active context through the `ticket-file` adapter.
2. Determine the earliest incomplete or deliberately reopened phase from stored content and phase state. Do not re-run completed phases.
3. Report the exact ticket ID, title, stored status, current phase, completed phases, open questions, and next gate.
4. If the same request explicitly asks for further permitted work, continue from that phase. Otherwise stop after the report and use the persisted-task controller for later input.

Never use this controller for a runtime-memory task.
