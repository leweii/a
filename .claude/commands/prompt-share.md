# /prompt:share <name> (project)

Share a local prompt with your team by copying it to the shared folder.

## Arguments

- `name` (required): The name of the local prompt to share (without .md extension)

## Instructions

When the user runs `/prompt:share <name>`, perform the following steps:

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

### 2. Check Local Prompt Exists

Verify the prompt exists in the local folder:

```bash
cat <paths.local>/<name>.md
```

If not found, display:
> Error: Local prompt '<name>' not found.
>
> Available local prompts:
> - prompt-1
> - prompt-2
>
> Use `/prompt:save <name> <description>` to create a new prompt first.

### 3. Check for Existing Shared Prompt

Check if a prompt with the same name already exists in shared:

```bash
ls <paths.shared>/<name>.md 2>/dev/null
```

If exists, ask for confirmation:
> A shared prompt named '<name>' already exists. Overwrite? (y/n)

### 4. Copy to Shared Folder

Copy the prompt from local to shared:

```bash
cp <paths.local>/<name>.md <paths.shared>/<name>.md
```

### 5. Confirm Success

On success, display:
> Prompt shared: `.claude/prompts/shared/<name>.md`
>
> To share with your team:
> ```
> git add .claude/prompts/shared/<name>.md
> git commit -m "Add shared prompt: <name>"
> git push
> ```

## Error Handling

- **Missing config**: Instruct user to create `.claude/prompt-config.json`
- **Prompt not found**: List available local prompts
- **Copy error**: Display the error message
- **Invalid name**: Reject names with special characters

## Examples

Share a local prompt:
```
/prompt:share code-review
```

Result:
```
Prompt shared: .claude/prompts/shared/code-review.md

To share with your team:
  git add .claude/prompts/shared/code-review.md
  git commit -m "Add shared prompt: code-review"
  git push
```
