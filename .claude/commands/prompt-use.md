# /prompt:use <name>

Fetch a prompt from the shared GitHub repository and inject it into the current session.

## Arguments

- `name` (required): The name of the prompt to fetch (without .md extension)

## Instructions

When the user runs `/prompt:use <name>`, perform the following steps:

### 1. Validate Environment

First, check that required environment variables are set:

```bash
gh auth status
```

If not authenticated, instruct the user to run `gh auth login`.

Check that `PROMPT_REPO_OWNER` and `PROMPT_REPO_NAME` are set. If not, display an error:
> Error: Missing required environment variables. Please set PROMPT_REPO_OWNER and PROMPT_REPO_NAME.

### 2. Fetch Prompt Content

Retrieve the prompt file from the repository:

```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts/<name>.md --jq '.content' | base64 -d
```

### 3. Handle Not Found

If the prompt doesn't exist, fetch the list of available prompts and suggest alternatives:

> Prompt '<name>' not found.
>
> Available prompts:
> - code-review
> - api-design
> - error-handling
>
> Use /prompt:list for full descriptions.

### 4. Parse and Inject

Extract the content after the YAML frontmatter (everything after the closing `---`).

Display confirmation:
> Loaded prompt: <name>
> Description: <description from frontmatter>

Then inject the prompt content into the session context so it guides subsequent interactions.

### 5. Progressive Disclosure

The prompt is only fully loaded when explicitly requested via this command. This ensures:
- Users discover prompts through `/prompt:list` first
- Full instructions are loaded on-demand, not preemptively
- Session context isn't cluttered with unused prompts

## Error Handling

- **Authentication error**: Instruct user to run `gh auth login`
- **Missing env vars**: List which variables need to be set
- **Prompt not found**: Display available alternatives
- **API error**: Display the error message from GitHub
- **Invalid name**: Reject names with special characters

## Example

User: `/prompt:use code-review`

Result: Fetches the code-review prompt from the repository and injects its instructions into the current session.

```
Loaded prompt: code-review
Description: Guidelines for reviewing pull requests

The following instructions are now active in this session:
───────────────────────────────────────────────────────────
### Requirement: Code Review Guidelines
Code reviews SHALL follow these guidelines to ensure consistency...
───────────────────────────────────────────────────────────
```
