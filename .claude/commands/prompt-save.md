# /prompt:save <name>

Save the current task context as a reusable prompt to the shared GitHub repository.

## Arguments

- `name` (required): The name for the prompt file (without .md extension)

## Instructions

When the user runs `/prompt:save <name>`, perform the following steps:

### 1. Validate Environment

First, check that required environment variables are set:

```bash
gh auth status
```

If not authenticated, instruct the user to run `gh auth login`.

Check that `PROMPT_REPO_OWNER` and `PROMPT_REPO_NAME` are set. If not, display an error:
> Error: Missing required environment variables. Please set PROMPT_REPO_OWNER and PROMPT_REPO_NAME.

### 2. Summarize Context

Analyze the current conversation and extract:
- The core task or goal
- Background context and constraints
- Step-by-step instructions
- Expected outcomes

### 3. Generate Task Description Prompt

Create a prompt file with this structure:

```markdown
---
description: <Brief one-line description of what this prompt does>
tags: [<relevant>, <tags>]
created: <YYYY-MM-DD>
---

# Task: <Descriptive Task Name>

## Context
<Background information, why this task exists, relevant constraints>

## Instructions
<Step-by-step instructions or guidelines for completing the task>

## Expected Outcome
<What should be achieved when following this prompt>
```

Notes:
- Sections may be omitted if not applicable
- Additional sections may be added as needed (e.g., `## Examples`, `## Notes`)
- Keep the format simple and readable

### 4. Check for Existing File

Query if the file already exists to get its SHA:

```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts/<name>.md --jq '.sha' 2>/dev/null
```

### 5. Upload to Repository

Base64 encode the content and upload via GitHub API:

For new files:
```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts/<name>.md \
  --method PUT \
  --field message="Add prompt: <name>" \
  --field content="<base64-encoded-content>"
```

For existing files (include SHA):
```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts/<name>.md \
  --method PUT \
  --field message="Update prompt: <name>" \
  --field content="<base64-encoded-content>" \
  --field sha="<existing-sha>"
```

### 6. Confirm Success

On success, display:
> Prompt saved successfully: https://github.com/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/blob/main/prompts/<name>.md

## Error Handling

- **Authentication error**: Instruct user to run `gh auth login`
- **Missing env vars**: List which variables need to be set
- **API error**: Display the error message from GitHub
- **Invalid name**: Reject names with special characters (allow alphanumeric, hyphens, underscores)

## Example

User: `/prompt:save code-review`

Result: Saves a prompt file summarizing the current task as `prompts/code-review.md` in the configured repository.
