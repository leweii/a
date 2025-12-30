# /prompt:list

List all available prompts from the shared GitHub repository.

## Instructions

When the user runs `/prompt:list`, perform the following steps:

### 1. Validate Environment

First, check that required environment variables are set:

```bash
gh auth status
```

If not authenticated, instruct the user to run `gh auth login`.

Check that `PROMPT_REPO_OWNER` and `PROMPT_REPO_NAME` are set. If not, display an error:
> Error: Missing required environment variables. Please set PROMPT_REPO_OWNER and PROMPT_REPO_NAME.

### 2. Fetch Directory Listing

Query the prompts directory:

```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts --jq '.[] | select(.type == "file") | .name'
```

### 3. Fetch and Parse Each Prompt

For each `.md` file found, fetch its content and extract the description from YAML frontmatter:

```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts/<filename> --jq '.content' | base64 -d
```

Parse the YAML frontmatter to extract the `description` and `tags` fields.

### 4. Display Results

Format the output as a table:

```
Available Prompts
─────────────────────────────────────────────────────────────────────
Name                 Description                          Tags
─────────────────────────────────────────────────────────────────────
code-review          Guidelines for reviewing PRs         [review, quality]
api-design           REST API design patterns             [api, design]
error-handling       Consistent error handling            [errors, patterns]
─────────────────────────────────────────────────────────────────────

Use /prompt:use <name> to load a prompt into your session.
```

### 5. Handle Empty Repository

If no prompts are found, display:
> No prompts found in the repository.
>
> Use /prompt:save <name> to create your first prompt.

## Error Handling

- **Authentication error**: Instruct user to run `gh auth login`
- **Missing env vars**: List which variables need to be set
- **Directory not found**: Display message that prompts directory doesn't exist yet
- **API error**: Display the error message from GitHub

## Example

User: `/prompt:list`

Result: Displays a formatted list of all available prompts with their descriptions.
