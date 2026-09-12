# Phase 1 — Discovery

Read [`../task-manager.md`](../task-manager.md) first. This file contains only Phase 1 behavior.

## Purpose

Establish the source and project facts that later phases depend on. Gather evidence without turning it into a specification.

## Inputs

- The active task context and its Task Source.
- `~/.dev-workflow/development.md`.

## Permissions

Write section **1. Discovery** of the active context.

## Steps

1. Read the project environment, repository instructions, and initialized active context.
2. Read all evidence already in Task Source, including the original user wording.
3. Fetch any source locator present in Task Source and append its available title, status, priority, description, comments, links, relationships, and attachments to the evidence bundle.
4. List every discovered external URL under **External Resources** without interpreting it yet.
5. Fill **Context** and **Linked Tickets** from evidence. Use `None` for unavailable optional values; do not invent them. Use the active task's Name as its title only when no authoritative title was supplied, and use `To Do` when a newly initiated task has no prior status.
6. Retain raw requirements, constraints, supplied acceptance criteria, resource URLs, and dependencies in Task Source for Phase 2.

## Exit gate

Continue only when **Context** is populated, relationships have a real value or `None`, and every discovered external URL is listed.

## Phase completion

Apply the shared phase-completion protocol and retain the updated active context.
