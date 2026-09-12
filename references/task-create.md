# Creating a Task

This is the controller for a new task. It is also the sole controller for every later same-runtime action on an ad-hoc task.

## Instruction set

Use the active-context model and adapter rules in [`task-manager.md`](task-manager.md), create missing shared project configuration through [`development-manager.md`](development-manager.md), and use [`task-profile-template.md`](task-profile-template.md) only when initializing the persisted ticket adapter.

## Procedure

1. **Capture the initiation.** Preserve the full raw user request in Task Source immediately. Resolve ticket precedence and select the adapter. If a non-ticket request has no actionable requirements, ask for them and do not initialize yet.
2. **Name the task.** Keep an exact ticket token as the persisted identity, or derive a short descriptive runtime label from the requested outcome. Never turn a runtime label into an ID or path.
3. **Ensure project context.** Load the shared environment, creating it only if missing. Never place per-task ad-hoc content in it.
4. **Initialize the active context.** Invoke the selected adapter once: create the exact ticket file when using persisted storage, or initialize sections 1–5 directly in runtime memory. Tell the user the task name and whether its context is persisted or runtime-only.
5. **Run the common SDD sequence.** Starting with the earliest incomplete phase, invoke Phases 1, 2, and 3 in order against the active context and generic Task Source. Retain each successful phase through the adapter. Phases 2 and 3 populate Testing Plan entries, but do not execute Phase 5 at this stage.
6. **Honor the implementation gate.** Present the Specification, Design, Progress checklist, and Testing Plan, then stop. Do not enter Phase 4 until a separate user message explicitly asks to implement.
7. **Resume coherently.** On later input, apply the manager's continuation rules. For a runtime-memory task, remain in this controller for questions, changes, Phase 4, Phase 5, and completion; never route it through another scenario. For a newly created persisted task, later activity may use the persisted-task controller selected by the skill.

When agent delegation is available, the same agent may run the common creation sequence as one unit. Pass the adapter, raw Task Source, and active context directly; require the updated context back, and enforce the same Phase 3 stop.
