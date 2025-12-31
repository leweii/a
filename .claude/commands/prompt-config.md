# /prompt:config <repo_owner> <repo_name>

Configure the GitHub repository settings for prompt storage and sharing.

## Arguments

- `repo_owner` (required): The GitHub username or organization that owns the repository
- `repo_name` (required): The name of the GitHub repository for prompt storage

## Instructions

When the user runs `/prompt:config <repo_owner> <repo_name>`, perform the following steps:

### 1. Parse Arguments

Extract:
- `repo_owner`: The first argument (GitHub username or organization)
- `repo_name`: The second argument (repository name)

If either argument is missing, ask the user:
> Please provide both the repository owner and name. Example:
> `/prompt:config myorg shared-prompts`

### 2. Validate Arguments

Ensure the arguments are valid:
- `repo_owner`: Must be alphanumeric with hyphens allowed (GitHub username format)
- `repo_name`: Must be alphanumeric with hyphens and underscores allowed (GitHub repo format)

If validation fails, display an error with the specific issue.

### 3. Update Configuration

Write the configuration to `prompt-config.json` in the project root:

```json
{
  "repo_owner": "<repo_owner>",
  "repo_name": "<repo_name>",
  "configured_at": "<ISO 8601 timestamp>"
}
```

### 4. Ensure Directory Structure

Create the prompt directories if they don't exist:

```bash
mkdir -p .claude/prompts/local
mkdir -p .claude/prompts/shared
```

### 5. Confirm Success

On success, display:
> Configuration saved to `prompt-config.json`
>
> Repository: `<repo_owner>/<repo_name>`
>
> You can now use:
> - `/prompt:save <name> <description>` - Save a new prompt locally
> - `/prompt:list` - List available prompts
> - `/prompt:share <name>` - Share a prompt with your team
> - `/prompt:publish` - Publish shared prompts to GitHub

## Error Handling

- **Missing arguments**: Prompt user to provide both repo_owner and repo_name
- **Invalid repo_owner**: Must match GitHub username format (alphanumeric and hyphens)
- **Invalid repo_name**: Must match GitHub repository format (alphanumeric, hyphens, underscores)
- **Write error**: Display the error message

## Examples

```
/prompt:config mycompany team-prompts
```
Result: Configures the repository to `mycompany/team-prompts`

```
/prompt:config leweii a
```
Result: Configures the repository to `leweii/a`
