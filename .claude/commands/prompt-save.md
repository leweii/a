# /prompt:save <name> <description> (project)

Generate and save a prompt based on the provided description.

## Arguments

- `name` (required): The name for the prompt file (without .md extension)
- `description` (required): A description of what the prompt should do. Claude will generate the full prompt content based on this description.

Prompts are saved to the **local** folder by default (private, not shared).
Use `/prompt:share <name>` to share a prompt with your team.

## Instructions

When the user runs `/prompt:save <name> <description>`, perform the following steps:

### 1. Parse Arguments

Extract:
- `name`: The first argument (prompt file name)
- `description`: Everything after the name (the prompt description)

If description is missing, ask the user:
> Please provide a description for the prompt. Example:
> `/prompt:save code-review Guidelines for reviewing PRs with focus on security and performance`

### 2. Load Configuration

Read the repository configuration from `.claude/prompt-config.json`:

```bash
cat .claude/prompt-config.json
```

Extract the paths configuration:
- `paths.local`: Local prompts folder (e.g., `.claude/prompts/local`)

If the config file doesn't exist, display an error:
> Error: Missing configuration. Please create `.claude/prompt-config.json` first.

### 3. Generate Prompt Content

Based on the user's description, generate a comprehensive prompt with this structure:

```markdown
---
description: <Brief one-line description based on user input>
tags: [<relevant>, <tags>]
created: <YYYY-MM-DD>
---

# Task: <Descriptive Task Name>

## Context
<Background information inferred from the description>

## Instructions
<Detailed step-by-step instructions generated based on the description>

## Expected Outcome
<What should be achieved when following this prompt>
```

**Generation Guidelines:**
- Expand the user's description into detailed, actionable instructions
- Infer relevant context and constraints
- Add practical examples where helpful
- Keep the tone professional and clear
- Generate appropriate tags based on the content

### 4. Save the Prompt

Save to the local folder:

```bash
# Write content to: <paths.local>/<name>.md
```

### 5. Confirm Success

On success, display:
> Prompt saved: `.claude/prompts/local/<name>.md`
>
> Use `/prompt:use <name>` to load this prompt.
> Use `/prompt:share <name>` to share with your team.

Then show a preview of the generated prompt content.

## Error Handling

- **Missing config**: Instruct user to create `.claude/prompt-config.json`
- **Missing description**: Ask user to provide a description
- **Invalid name**: Reject names with special characters (allow alphanumeric, hyphens, underscores)
- **Write error**: Display the error message

## Examples

```
/prompt:save code-review Guidelines for reviewing PRs focusing on security vulnerabilities and performance issues
```
Result: Generates a comprehensive code review prompt and saves to `.claude/prompts/local/code-review.md`

```
/prompt:save api-design Best practices for designing RESTful APIs with proper error handling and versioning
```
Result: Generates an API design prompt and saves to `.claude/prompts/local/api-design.md`
