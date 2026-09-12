# Development Manager

Manages `~/.dev-workflow/development.md`, the project environment file.

It defines the context, conventions, and boundaries within which every task specification is written. It **supplements** the project's own `AGENTS.md` and never replaces it — see *Authority order* among the hub's shared rules.

## File Template

See [`development-environment-template.md`](development-environment-template.md).

## Auto-Discovery

Before asking the user, infer each field using shell commands or whatever tools the host platform provides:

| Field | Discovery sources |
|-------|-------------------|
| `developer_name` | `git config user.name`, `whoami`, OS full name. |
| `developer_email` | `git config user.email`. |
| `codebases` | The current working directory and nearby directories containing `.git`. Use the directory name as the description. |
| `ticket_manager_url` | `package.json` `bugs.url` or `repository.url`; `.jira/`, `.linear/`, `docs/`; `README.md`; the `JIRA_URL` and `LINEAR_API_URL` environment variables. |
| `designer_url` | `.figma` files; Figma, Sketch, or Adobe URLs in `README.md`. |
| `custom_links` | URLs in `README.md` matching docs, wiki, staging, API, or dashboard patterns. Keep the 3 most relevant. |
| `additional_workflow` | `.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, or `CONVENTIONS.md`. Summarise into one sentence. |

## Creation Flow

1. Tell the user: *"Project config not found at ~/.dev-workflow/development.md. I'll auto-discover your project settings and create it."*
2. Run auto-discovery for every field.
3. Present the inferred values as `Field: Discovered Value (source)`.
4. Create `~/.dev-workflow/` if needed and write the file from the template.
5. For any field that could not be discovered, write `Unknown` rather than leaving a `{placeholder}`, and list those fields when you ask the user to review.
6. Confirm creation, summarise the final config, and ask the user to correct anything wrong or marked `Unknown`.
