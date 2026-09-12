# Phase 2 — Specification

Read [`../task-manager.md`](../task-manager.md) first. This file contains only Phase 2 behavior.

## Purpose

Define what must be built, why, and where its boundaries are before a technical plan exists.

## Inputs

- Section **1. Discovery**.
- The complete Task Source.
- `~/.dev-workflow/development.md`.

## Permissions

Write section **2. Specification** and the product-level entries of section 5's **Testing Plan**.

## Steps

1. Read Discovery and every item in Task Source.
2. Fetch and read every listed external resource.
3. Write precise, single-purpose **Requirements** grounded in the evidence.
4. Write observable **Acceptance Criteria**, preferring supplied criteria and filling only genuine gaps.
5. Add one product-level test scenario per criterion to the **Testing Plan**. Leave result fields empty for Phase 5.
6. Define **Scope**, **Out of Scope**, and **Depends on**, using `None` where appropriate.
7. Apply the shared unclear-source rule to contradictions, meaning-changing duplication, and ambiguity.
8. Present the completed specification to the user.

## Exit gate

Every subsection must contain real content. Presentation does not require approval, so Phase 3 may begin immediately unless an unresolved answer could change scope or design; in that case, stop and wait.

## Phase completion

Apply the shared phase-completion protocol. Present Requirements, Acceptance Criteria, boundaries, and unresolved questions exactly as retained.
