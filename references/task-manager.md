# Task Manager

This is the shared SDD engine. Scenario controllers establish an **active task context** and invoke phases; phases operate only on that context and its generic **Task Source**. Storage differences are handled here, never inside a phase.

## Contents

- [SDD workflow](#sdd-workflow)
- [Active task context](#active-task-context)
- [Storage adapters](#storage-adapters)
- [Task source](#task-source)
- [Phase index](#phase-index)
- [Execution and continuation](#execution-and-continuation)
- [Shared rules](#shared-rules)

## SDD workflow

**Spec-Driven Development** makes a structured specification the source of truth before code changes begin. Discovery establishes facts, Specification defines the contract, Design plans the solution, Implementation executes that plan, and Validation proves the result against the contract.

## Active task context

Maintain one active context with:

- **Name** — exact ticket token for a ticket-backed task; concise work-derived label otherwise.
- **Task Source** — the unmodified initiating requirements plus any fetched ticket material, linked resources, and later user updates.
- **Adapter** — `ticket-file` or `runtime-memory`.
- **Sections 1–5** — Discovery, Specification, Design, Implementation, and Validation.
- **Phase state** — current phase, completed phases, open questions, and whether the explicit implementation gate has been satisfied. For persisted tasks, derive this state from completed sections and checklist results rather than adding a second record.

Capture the Task Source before running Discovery. A phase is complete only after its exit gate and the phase-completion protocol succeed.

## Storage adapters

All scenario and phase operations use `read active context`, `update active context`, and `retain active context`. Apply those operations through exactly one adapter:

### `ticket-file`

- Identity is the ticket token exactly as supplied.
- Persistent location is `~/.dev-workflow/{TASK-ID}.md`.
- On creation, initialize that file from the ticket profile template. On load, read the entire file before deciding the next phase.
- Every context update is written back to the same file.

### `runtime-memory`

- Identity is a concise descriptive name derived from the requested work.
- The complete context exists only in the current runtime/conversation memory.
- Never generate an ID or task-file path. Never create, search for, read, or update a task file, alternate bookkeeping file, cache, database entry, or persistent-memory record for it.
- Source code, tests, and other implementation artifacts remain normal project files. Shared project configuration may remain in `~/.dev-workflow/development.md`, but must not contain ad-hoc task bookkeeping.
- If delegated, transfer the context directly in the agent message and receive updates directly; do not use a file as an intermediary.

Adapter selection is complete before a phase starts. Phase instructions must not inspect the adapter or branch on task format.

## Task source

Task Source is the format-neutral evidence bundle consumed by every phase. It can contain raw user requirements, ticket fields, comments, linked materials, constraints, acceptance criteria, and user corrections. Missing optional metadata is `None`; no phase invents it.

The raw text that initiated a non-ticket task must remain available verbatim in Task Source for the life of the runtime context. Fetching and normalizing evidence may extend the bundle but must not replace that text.

## Phase index

Run phases in order. Section N is owned by Phase N except for the declared cross-section writes below.

| Phase | Instructions |
|---|---|
| **1. Discovery** | [`phases/phase-1-discovery.md`](phases/phase-1-discovery.md) |
| **2. Specification** | [`phases/phase-2-specification.md`](phases/phase-2-specification.md) |
| **3. Design** | [`phases/phase-3-design.md`](phases/phase-3-design.md) |
| **4. Implementation** | [`phases/phase-4-implementation.md`](phases/phase-4-implementation.md) |
| **5. Validation** | [`phases/phase-5-validation.md`](phases/phase-5-validation.md) |

Before running a phase, read this manager and then that phase's instructions.

## Execution and continuation

1. Start at the earliest incomplete phase and run phases in order.
2. Never re-run a completed phase merely because a task was loaded or resumed.
3. Phase 3 always stops at its explicit implementation gate. Phase 4 requires a later, separate user instruction to implement.
4. Phase 5 follows Phase 4 only after the Phase 4 exit gate succeeds. It is never prepared or executed before Implementation.
5. After each phase or material update, retain the active context through its adapter.

For a later user update, append the exact update to Task Source, then identify the earliest affected phase. Reopen that phase and invalidate only affected downstream conclusions, checklist items, or tests; preserve unrelated completed work. Update Specification and Design before continuing Implementation whenever scope or behavior changed.

Runtime-memory tasks remain under the *Create* controller for questions, requirement changes, explicit implementation, validation, and completion. Persisted ticket tasks use the *Load* controller to restore context and the *Work* controller for later activity.

If runtime memory is unavailable, no active context exists. Ask the user for the original requirements and current progress, rebuild a new runtime context from that response, and clearly state what could not be recovered. Do not probe the filesystem for it.

## Shared rules

### Ask when a source is unclear

A source is unclear when it is contradictory, duplicated in a way that changes meaning, or too vague to act on. Never silently choose a reading:

1. Record each reading, the provisional choice and reason, and answer state under **Conflicts & Open Questions** in section 2.
2. Ask the user, quoting the unclear text and naming its source.
3. Stop when the answer could change scope or design; otherwise continue with the recorded provisional choice.
4. On resolution, update every affected section before implementation continues.

### Cross-section writes

A phase writes only its owned section plus these declared exceptions:

- Every phase updates **Last Update** under section 1.
- Every phase may append **Conflicts & Open Questions** under section 2.
- Phase 2 adds product scenarios to section 5's **Testing Plan**.
- Phase 3 writes section 4's **Progress** checklist and adds technical cases to the **Testing Plan**.
- Phase 5 may set section 1's **Status** to `Done`, but only after every validation exit condition has passed.

### Timestamps and phase completion

Use UTC ISO-8601 timestamps with a `Z` suffix. At the end of a phase:

1. Update its owned context so it reflects reality.
2. Remove every `{curly-brace}` placeholder in the completed section, using `None` where appropriate.
3. Set **Last Update** to the current timestamp.
4. Retain the active context through its adapter.

### One checklist

The only checklist is **Progress** in section 4. Do not add a second task list elsewhere.

### External resources

Use the host's supported connector, web, browser, or repository tools to read required sources. If a source is inaccessible, ask the user to paste or summarize the relevant content; do not guess or skip it silently.

### Authority order

When project conventions disagree, apply this order:

1. The local `AGENTS.md`.
2. Local `CONTRIBUTING.md`, `CONVENTIONS.md`, or `.github/PULL_REQUEST_TEMPLATE.md`.
3. `~/.dev-workflow/development.md`.

Record overrides in **Implementation Notes**. Conflicts not settled by this order use the unclear-source rule.
