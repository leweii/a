# /prompt:list (project)

List all available prompts from local and shared folders.

## Arguments

- `--local` or `-l` (optional): Show only local prompts
- `--shared` or `-s` (optional): Show only shared prompts

By default, shows prompts from both folders.

## Instructions

When the user runs `/prompt:list`, perform the following steps:

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

### 2. List Local Prompts

If not filtered to shared-only, list all `.md` files in the local folder:

```bash
ls -1 <paths.local>/*.md 2>/dev/null
```

For each file found, read and parse the YAML frontmatter to extract `description` and `tags`.

### 3. List Shared Prompts

If not filtered to local-only, list all `.md` files in the shared folder:

```bash
ls -1 <paths.shared>/*.md 2>/dev/null
```

For each file found, read and parse the YAML frontmatter to extract `description` and `tags`.

### 4. Display Results

Format the output as a table, grouped by type:

```
Available Prompts
═══════════════════════════════════════════════════════════════════════

Local Prompts (private)
───────────────────────────────────────────────────────────────────────
Name                 Description                          Tags
───────────────────────────────────────────────────────────────────────
code-review          Guidelines for reviewing PRs         [review, quality]
my-notes             Personal task notes                  [notes]

Shared Prompts (synced via git)
───────────────────────────────────────────────────────────────────────
Name                 Description                          Tags
───────────────────────────────────────────────────────────────────────
api-design           REST API design patterns             [api, design]
═══════════════════════════════════════════════════════════════════════

Commands:
  /prompt:use <name>              Load a prompt
  /prompt:save <name> <desc>      Create a new local prompt
  /prompt:share <name>            Share a local prompt
```

### 5. Handle Empty Folders

If a folder is empty or doesn't exist, show:

**For local:**
> No local prompts found.

**For shared:**
> No shared prompts found.

If both are empty:
> No prompts found.
>
> Use `/prompt:save <name> <description>` to create your first prompt.

## Error Handling

- **Missing config**: Instruct user to create `.claude/prompt-config.json`
- **Directory not found**: Display message that prompts directory doesn't exist yet
- **Read error**: Display the error message

## Examples

List all prompts:
```
/prompt:list
```

List only local prompts:
```
/prompt:list --local
```

List only shared prompts:
```
/prompt:list --shared
```
