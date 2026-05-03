# Development Manager

Manages the `~/.dev-workflow/development.md` project environment file.

## File Template

```markdown
# Development Environment

## Developer
- **Name**: {developer_name}
- **Email**: {developer_email}

## Ticket Manager
- **URL**: {ticket_manager_url}

## Designer
- **URL**: {designer_url}

## Codebases
- **{description}**: {local_path}

## Custom Links
- **{description}**: {url}

## Additional Workflow
{branch naming, commit conventions, coding standards, or other project rules}
```

## Auto-Discovery

Before asking the user, attempt to infer each field using Shell commands:

| Field | Discovery Sources |
|-------|-------------------|
| `developer_name` | `git config user.name`, `whoami`, OS full name. |
| `developer_email` | `git config user.email`. |
| `codebases` | Current working directory and nearby directories containing `.git`. Use directory name as description. |
| `ticket_manager` | Check `package.json` `bugs.url` or `repository.url`; scan `.jira/`, `.linear/`, `docs/`; check `README.md`; inspect `JIRA_URL`, `LINEAR_API_URL`. |
| `designer` | Scan for `.figma` files; search `README.md` for Figma/Sketch/Adobe URLs. |
| `custom_links` | Extract URLs from `README.md` matching docs, wiki, staging, API, or dashboard patterns. Limit to 3 most relevant. |
| `additional_workflow` | Check `.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, or `CONVENTIONS.md`. Summarize into one sentence. |

## Creation Flow

When `development.md` is missing:

1. Inform the user: *"Project config not found at ~/.dev-workflow/development.md. I'll try to discover your project settings first."*
2. Run auto-discovery for every field.
3. Present the inferred values in a structured list: `Field: Discovered Value (source)`.
4. Ask the user to confirm, correct, or fill in missing values.
5. Create `~/.dev-workflow/` if needed and write the file from the template.
6. Confirm creation and summarize the final config.

After loading:
- Reference the **Codebases** section to scope file operations.
- Apply **Additional Workflow** rules to every task.
