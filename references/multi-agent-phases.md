# Multi-Agent Phase Mapping

This reference describes how to distribute the SDD workflow across specialized agents when the host platform supports multiple agents or planning/build modes.

The guidance below is platform-agnostic. Use the agent mechanisms available in your environment (subagents, planning modes, read-only agents, etc.) to implement the same separation of concerns.

## Default mapping

| SDD Phase | Agent role | Permissions | Output |
|-----------|-----------|-------------|--------|
| **1. Discovery** | **Explorer / Research** | Read-only on project files and external resources | Populated Context and Linked Tickets in the task file. |
| **2. Specify** | **Analyst / Product** | Write only to the task file specification sections | Requirements, Acceptance Criteria, Boundaries & Dependencies. |
| **3. Design** | **Architect / Planner** | Read-only on source code; write only to the task file design sections | Technical Design, File Structure Plan, Task Breakdown. |
| **4. Implement** | **Builder / Coder** | Read-write on source files and task file progress | Implemented code, updated Progress and Implementation Notes. |
| **5. Validate & Archive** | **Reviewer / QA** | Read source files and task file; may run commands/tests | Testing Plan results, Spec Compliance check, archived task file. |

## Principles

- **Least privilege**: Discovery and Design agents should not modify source code.
- **Separation of concerns**: The agent that writes the spec should not be the same one that implements it, to reduce self-justification bias.
- **Independent validation**: A different agent from the Builder should verify the work against the spec.
- **Progressive disclosure**: Each agent reads only the parts of the task file relevant to its phase.

## Handling different agent capabilities

| Environment capability | How to apply this mapping |
|------------------------|---------------------------|
| **Subagents available** | Spawn the appropriate agent type for each phase and pass only the relevant context. |
| **Planning mode available** | Use it for Discovery, Specify, and Design. Switch to implementation mode only after the plan is approved. |
| **Read-only mode available** | Use it for Discovery and Design; switch to read-write only for Implement and Validate. |
| **Single-agent environment** | A single agent can run all phases. Explicitly pause for user approval at the end of Design before starting Implement. |

## Transition gates

- Do not proceed from **Specify** to **Design** until the specification is complete.
- Do not proceed from **Design** to **Implement** until the user approves the plan.
- Do not proceed from **Implement** to **Validate** until the Builder confirms all tasks are done.
- Do not archive the task file until the Reviewer confirms all acceptance criteria are met.
