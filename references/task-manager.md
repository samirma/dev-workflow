# Task Manager

Manages per-task files in `~/.dev-workflow/` named after ticket codes, following a **Spec-Driven Development (SDD)** workflow.

## SDD Workflow Overview

This skill implements a 5-phase SDD (Spec-Driven Development) workflow. SDD is a software engineering methodology where structured specifications act as the primary source of truth before any code is written. Specifications become executable contracts that guide implementation and validate correctness:

1. **Discovery** — Understand the ticket and project context.
2. **Specify** — Define requirements, acceptance criteria, and boundaries.
3. **Design** — Create technical plan, file structure, and task breakdown.
4. **Implement** — Execute tasks with validation checkpoints.
5. **Validate & Archive** — Verify against the spec and complete.

---

## File Naming

```
~/.dev-workflow/{TICKET-CODE}.md
```

Use the exact ticket code from the ticket manager. For ad-hoc tasks, use `adhoc-{timestamp}.md`.

## File Template

See [`task-profile-template.md`](task-profile-template.md) for the task profile template.

---

## Phase 1: Discovery

When creating or loading a task file:

1. Load `development.md` to understand project context, codebases, and workflow rules.
2. If the task file does not exist, create it from the template.
3. For tickets with a URL, use the browser tool to extract:
   - Title, Priority, Status
   - Description and comments (including any inline URLs to external docs or designs)
   - Linked tickets (blockers, related)
   - Attachments
4. Populate the **Context** and **Linked Tickets** sections.
5. Set `Last Update` to the current ISO timestamp.

---

## Phase 2: Specify

> Define the contract before writing code.

Before any design or implementation:

1. Read the ticket description carefully. If the description or comments contain URLs (Confluence pages, Figma designs, API specs, architecture docs, etc.), use the browser tool to fetch and read them.
2. Extract or write **Requirements** — define *what* must be built and *why*.
3. Extract or write **Acceptance Criteria** — define "done" from a user or product perspective.
4. Define **Boundaries & Dependencies**:
   - What is explicitly in scope.
   - What is explicitly out of scope.
   - Dependencies on other tasks or systems.
5. Do not proceed to Design until the specification is complete.
   - In **non-interactive** mode, write the specification and proceed.
   - In **interactive** mode, ask the user: *"Do you want me to create the initial specification (requirements, acceptance criteria, and boundaries) based on the ticket description?"*

### Guidelines for Specification

- **Requirements** should answer *how to fix or solve* the ticket. Base them on:
  - The ticket description.
  - Attached or linked resources.
  - Relevant information in comments.
- **Acceptance Criteria** should define what "done" looks like from a user or product perspective. Extract directly from the ticket description.
- Keep requirements simple and precise. One idea per bullet.

---

## Phase 3: Design

> Create the technical plan and task breakdown.

After specification is complete:

1. Analyze the project codebase to understand current architecture and patterns.
2. Write **Technical Design** — architecture decisions, approach, patterns to use.
3. Create **File Structure Plan** — list files to create, modify, or delete.
4. Break work into **Task Breakdown** — small, actionable tasks (2–15 minutes each).
   - Each task should have clear boundaries.
   - Annotate dependencies between tasks (e.g., `_Depends:_ task-1`).
5. Do not proceed to Implementation until the design is complete.

---

## Phase 4: Implement

> Execute the plan and track progress. **Only run this phase when explicitly asked by the user.**

After the user explicitly requests implementation:

1. Work through the **Task Breakdown** sequentially.
2. Update **Progress** as tasks are completed.
3. Capture **Implementation Notes** — learnings, decisions, deviations from plan.
4. Follow project **Additional Workflow** rules from `development.md`.
5. Apply **TDD** (Test-Driven Development) where appropriate:
   - **RED** — Write a failing test.
   - **GREEN** — Write minimal code to make it pass.
   - **REFACTOR** — Clean up while keeping tests green.
6. Respect **Boundaries** — do not modify files or systems outside the defined scope without updating the spec first.

---

## Phase 5: Validate & Archive

> Verify against the specification before completing.

Before marking a task complete:

1. Run all **Testing Plan** steps — unit tests, manual verification, integration tests.
2. Perform **Boundary Check** — verify no unintended changes outside scope.
3. Perform **Spec Compliance** — verify all **Acceptance Criteria** are met.
4. Update **PR Status** with final state.
5. Update `Last Update` timestamp.
6. Move the task file to `~/.dev-workflow/archive/{TICKET-CODE}.md` to keep the active workspace clean.
7. Inform the user the task is archived and complete.

---

## Updating Requirements

When the user provides new information, constraints, or scope changes:

1. Return to **Phase 2: Specify** — update Requirements, Acceptance Criteria, or Boundaries.
2. If the change affects the plan, return to **Phase 3: Design** — update Technical Design and Task Breakdown.
3. Preserve existing requirements and tests that are still valid.
4. Update `Last Update` timestamp.
5. Confirm the update to the user.

**Rule**: Do not proceed with implementation until changes are captured in the specification.

---

## Update from Ticket Manager

When the user asks to check or update ticket or PR status:

1. Construct the ticket URL from the **Ticket Manager** section of `development.md` + ticket code.
2. Use the browser tool to open the ticket URL and extract the current status, title, and latest comments.
3. Compare live information with the stored task context.
4. If the page requires authentication that cannot be satisfied, fall back to asking the user: *"Please open the ticket and tell me what has changed (status, comments, review feedback)."*
5. Update the task file with the new information and update `Last Update`.
6. Summarize what changed.
