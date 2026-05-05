# Task Manager

Manages per-task files in `~/.dev-workflow/` named after ticket codes.

## File Naming

```
~/.dev-workflow/{TICKET-CODE}.md
```

Use the exact ticket code from the ticket manager. For ad-hoc tasks, use `adhoc-{timestamp}.md`.

## File Template

See [`task-profile-template.md`](task-profile-template.md) for the task profile template.

## Guidelines

### Technical Requirements

The purpose of the **Technical Requirements** section is to define how to fix or solve the ticket. Base this on:
- The ticket description.
- Attached or linked resources.
- Relevant information in comments.

Always consider the current project code to give detailed, file-specific requirements.

### Acceptance Criteria

The purpose of the **Acceptance Criteria** section is to define what "done" looks like from a user or product perspective. Extract this directly from the ticket description.

### Testing

The purpose of the **Testing** section is to define clear steps to evaluate and verify that the ticket is properly resolved.

## New Task

When creating a task file that does not already exist:

1. Create the file from the template using `development.md`:
   - Fill `{TICKET-CODE}` and `{ticket_manager}`.
   - Set `Last Update` to the current ISO timestamp.
   - Leave **Technical Requirements**, **Acceptance Criteria**, **Testing**, **Ticket Manager Status** comments, and **PR Status** empty.
2. Ask the user: *"Do you want me to create initial Technical Requirements and Testing based on the ticket description?"*
   - In **non-interactive** mode, proceed as if the answer is yes.
3. If the user agrees (or non-interactive):
   - Use the browser tool to open the ticket URL.
   - Extract **Title**, **Priority**, and **Acceptance Criteria** from the ticket page.
   - Extract any **linked tickets** (blockers or related) if visible.
   - Investigate the ticket description and all linked resources (URLs, attachments, comments).
   - Read carefully the ticket description and the project code to acuarally write Technical Requirements to address the described issue
   - Define **Testing** with step-by-step validation: unit tests to implement and/or manual user interactions, depending on the case.
   - Populate the task file with all derived fields.
4. If the user declines (interactive only):
   - Leave **Technical Requirements**, **Acceptance Criteria**, and **Testing** empty.
   - Proceed with the user's request and populate requirements from the request text as work progresses.

## Updating Requirements

When the user provides new information, constraints, or scope changes:

1. Append or edit the **Technical Requirements** section. Update **Acceptance Criteria** and **Testing** if affected.
2. Keep requirements simple and precise. One idea per bullet.
3. Preserve existing requirements and tests that are still valid.
4. Update `Last Update` timestamp.
5. Confirm the update to the user.

**Rule**: Do not proceed with implementation until the user's latest request is captured as a requirement.

## Update from Ticket Manager

When the user asks to check or update ticket or PR status:

1. Construct the ticket URL from the **Ticket Manager** section of `development.md` + ticket code.
2. Use the browser tool to open the ticket URL and extract the current status, title, and latest comments.
3. Compare live information with the stored `ticket_manager_status` and `pr_status`.
4. If the page requires authentication that cannot be satisfied, fall back to asking the user: *"Please open the ticket and tell me what has changed (status, comments, review feedback)."*
5. Update the task file with the new information and update `Last Update`.
6. Summarize what changed.

## Completion

When the task is finished:

1. Ensure all **Technical Requirements** and **Acceptance Criteria** are satisfied and all **Testing** steps pass or are validated.
2. Update **Ticket Manager Status** and **PR Status** to their final states.
3. Update `Last Update`.
4. Move the task file to `~/.dev-workflow/archive/{TICKET-CODE}.md` to keep the active workspace clean.
5. Inform the user the task file is archived and up to date.


