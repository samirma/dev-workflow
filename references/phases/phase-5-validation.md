# Phase 5 — Validation

Read [`../task-manager.md`](../task-manager.md) first. This file contains only Phase 5 behavior.

## Purpose

Verify the implementation against the specification and close the task only on evidence.

## Inputs

- Section **2. Specification**.
- Section **4. Implementation**.
- Section 5's **Testing Plan**.
- The working tree and project test commands.

## Permissions

Run commands and tests without editing source. Write section **5. Validation** and, after all success gates pass, section 1's **Status**.

## Steps

1. Read Acceptance Criteria, Boundaries & Dependencies, Conflicts & Open Questions, Progress, and Implementation Notes.
2. Run every product and technical Testing Plan entry. Record the actual result, including failures. Add a missing essential check as `(added in Phase 5)`.
3. Perform **Boundary Check** by inspecting the complete change set and recording what was reviewed.
4. Evaluate **Spec Compliance** criterion by criterion as `met` or `not met`, with evidence.
5. Record **Code Review** feedback and current **PR Status**, using `None` for unavailable fields.
6. Evaluate the exit gate. If any condition fails, retain the validation evidence without changing Status, report whether code or specification must return to an earlier phase, and stop.
7. Only after every exit condition succeeds, set section 1's **Status** to `Done` and complete the phase.

## Exit gate

Success requires every Acceptance Criterion to be verified `met`, a clean Boundary Check, no unresolved Conflicts & Open Questions, and completion of every required Testing Plan entry. A failed or unavailable required check does not pass.

## Phase completion

After the success gate, apply the shared phase-completion protocol. Present criterion-level compliance and boundary evidence, then tell the user the task is complete.
