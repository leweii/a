# /prompt:publish (project)

Publish all shared prompts to GitHub repository.

## Instructions

When the user runs `/prompt:publish`, perform the following steps:

### 1. Load Configuration

Read the repository configuration from `.claude/prompt-config.json`:

```bash
cat .claude/prompt-config.json
```

Extract:
- `paths.shared`: Shared prompts folder (e.g., `.claude/prompts/shared`)

If the config file doesn't exist, display an error:
> Error: Missing configuration. Please create `.claude/prompt-config.json` first.

### 2. Check for Changes

Check if there are any changes to publish:

```bash
git status --porcelain <paths.shared>/
```

If no changes, display:
> No changes to publish. Shared prompts are up to date.

### 3. Show Changes Summary

Display what will be published:

```bash
git status <paths.shared>/
```

Show a summary:
> Changes to publish:
> - Added: api-design.md
> - Modified: code-review.md

### 4. Stage Changes

Add all shared prompts to staging:

```bash
git add <paths.shared>/
```

### 5. Commit Changes

Create a commit with a descriptive message:

```bash
git commit -m "Update shared prompts

- <list of added/modified/deleted files>

🤖 Generated with Claude Code"
```

### 6. Push to Remote

Force push to main branch, overwriting remote:

```bash
git push origin main --force
```

### 7. Confirm Success

On success, display:
> Shared prompts published successfully!
>
> Committed and pushed:
> - api-design.md (added)
> - code-review.md (modified)
>
> Your team can now access these prompts by pulling the latest changes.

## Error Handling

- **Missing config**: Instruct user to create `.claude/prompt-config.json`
- **No changes**: Inform user that prompts are already up to date
- **Git not initialized**: Instruct user to initialize git repository
- **No remote**: Instruct user to add a remote repository
- **Push failed**: Display the error and suggest solutions (e.g., check permissions, check remote URL)
- **Uncommitted changes in other files**: Warn user and ask if they want to proceed with only prompt changes

## Example

```
/prompt:publish
```

Result:
```
Publishing shared prompts...

Changes detected:
  + code-review.md (new)
  ~ api-design.md (modified)

Committing changes...
Pushing to origin/main...

Shared prompts published successfully!

Your team can access these prompts by running:
  git pull
```
