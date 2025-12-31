# /prompt:use <name> (project)

Load a prompt and inject it into the current session.

## Arguments

- `name` (required): The name of the prompt to load (without .md extension)

The command searches local folder first, then shared folder. Local prompts take priority.

## Instructions

When the user runs `/prompt:use <name>`, perform the following steps:

### 1. Load Configuration

Read the repository configuration from `.claude/prompt-config.json`:

```bash
cat .claude/prompt-config.json
```

Extract the paths configuration:
- `paths.local`: Local prompts folder (e.g., `.claude/prompts/local`)
- `paths.shared`: Shared prompts folder (e.g., `.claude/prompts/shared`)

If the config file doesn't exist, display an error:
> Error: Missing configuration. Please create `.claude/prompt-config.json` first.

### 2. Search for Prompt

Search for the prompt file in this order (local takes priority):

1. First, check local folder: `<paths.local>/<name>.md`
2. If not found, check shared folder: `<paths.shared>/<name>.md`

```bash
# Check local first
if [ -f "<paths.local>/<name>.md" ]; then
  cat "<paths.local>/<name>.md"
# Then check shared
elif [ -f "<paths.shared>/<name>.md" ]; then
  cat "<paths.shared>/<name>.md"
fi
```

### 3. Handle Not Found

If the prompt doesn't exist in either folder, list available prompts:

> Prompt '<name>' not found.
>
> Available prompts:
>
> Local:
> - code-review
> - my-notes
>
> Shared:
> - api-design
>
> Use `/prompt:list` for full descriptions.

### 4. Parse and Inject

Extract the content after the YAML frontmatter (everything after the closing `---`).

Display confirmation with source indicator:

**For local prompts:**
> Loaded prompt: <name> (local)
> Description: <description from frontmatter>

**For shared prompts:**
> Loaded prompt: <name> (shared)
> Description: <description from frontmatter>

Then inject the prompt content into the session context so it guides subsequent interactions.

### 5. Display Loaded Content

Show the loaded prompt content:

```
───────────────────────────────────────────────────────────
<prompt content>
───────────────────────────────────────────────────────────
```

## Error Handling

- **Missing config**: Instruct user to create `.claude/prompt-config.json`
- **Prompt not found**: Display available alternatives from both folders
- **Read error**: Display the error message
- **Invalid name**: Reject names with special characters

## Example

```
/prompt:use code-review
```

Result:
```
Loaded prompt: code-review (local)
Description: Guidelines for reviewing pull requests

───────────────────────────────────────────────────────────
# Task: Code Review

## Instructions
Code reviews SHALL follow these guidelines...
───────────────────────────────────────────────────────────
```
