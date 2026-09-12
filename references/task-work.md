# Working on a Persisted Ticket Task

This controller handles later questions, source refreshes, requirement changes, implementation, and validation for an active ticket task whose context is persisted.

Read [`task-manager.md`](task-manager.md) once, then invoke only the current phase's indexed instructions.

## Continue work

1. Read the whole active context through the `ticket-file` adapter.
2. Apply any new user input to Task Source and use the continuation rules to reopen only affected work.
3. Resume the earliest incomplete phase. Do not re-run a completed phase without a recorded source change that affects it.
4. Enforce the Phase 3 explicit implementation gate. An approval without a separate implementation instruction does not enter Phase 4.
5. After Phase 4 succeeds, run Phase 5. Set `Done` only when its validation gates pass.

## Refresh from the ticket manager

When the user asks for a refresh, fetch the ticket and compare its fields, comments, links, and attachments with Task Source. Append changed evidence, report the differences, and identify the earliest affected phase before modifying downstream context. Preserve completed work that remains valid.

## Questions and conflicts

Record unclear-source entries in the active context before asking the user. When an answer arrives, append it to Task Source, update affected prior sections, and then resume at the correct phase.

Runtime-memory tasks never enter this controller.
