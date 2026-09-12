# Phase 4 — Implementation

Read [`../task-manager.md`](../task-manager.md) first. This file contains only Phase 4 behavior.

## Entry gate

Run only after a separate, explicit user instruction to implement has satisfied the Phase 3 gate.

## Purpose

Execute the approved plan while keeping active context accurate.

## Inputs

- Sections **2. Specification** and **3. Design**.
- Section 4's **Progress** checklist.
- **Additional Workflow** from the shared environment.

## Permissions

Modify source only inside listed codebases and declared Scope. In active context, update **Progress** and **Implementation Notes** in section 4.

## Steps

1. Read the Specification, Design, and Progress checklist.
2. Work through Progress in order and honor dependencies.
3. Tick an item only when complete. Do not silently rewrite, reorder, or delete planned items. If the plan is wrong, update the affected earlier sections before continuing.
4. Use RED–GREEN–REFACTOR where the change is testable.
5. Record implementation decisions, discoveries, deviations, and convention overrides in **Implementation Notes**.
6. Keep changes inside declared boundaries. A needed scope change must update Specification and Design first.
7. Follow the shared environment's workflow subject to the authority order.

## Exit gate

Continue only when every Progress item is ticked. An item may remain unticked only when its reason is recorded and the user explicitly accepts leaving it incomplete.

## Phase completion

Apply the shared phase-completion protocol. Summarize changed files, delivered behavior, deviations, and validation risks, then proceed to Phase 5.
