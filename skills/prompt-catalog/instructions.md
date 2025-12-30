# Prompt Catalog Skill Instructions

## Overview

This skill enables autonomous identification and suggestion of relevant prompts from the shared GitHub repository. Use semantic matching to connect user tasks with cataloged prompts.

## Operational Instructions

### 1. Context Analysis

When analyzing the user's current task, extract:
- **Task type**: code review, API design, testing, documentation, refactoring, etc.
- **Technology context**: languages, frameworks, tools being used
- **Intent signals**: keywords like "best practice", "standard", "template", "pattern"

### 2. Repository Query

Fetch the prompt catalog:

```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts --jq '.[] | select(.type == "file") | .name'
```

For each prompt, fetch metadata:

```bash
gh api repos/${PROMPT_REPO_OWNER}/${PROMPT_REPO_NAME}/contents/prompts/<filename> --jq '.content' | base64 -d | head -20
```

Extract from YAML frontmatter:
- `description`: What the prompt does
- `tags`: Categorization keywords

### 3. Semantic Matching

Match user context against prompt metadata:

| User Signal | Match Against |
|-------------|---------------|
| Task description | Prompt `description` field |
| File types being edited | Prompt `tags` array |
| Keywords in conversation | Prompt name and tags |
| Explicit requests | Exact name match |

**Match scoring:**
- Exact tag match: High relevance
- Partial description match: Medium relevance
- Related technology: Low relevance

### 4. Suggestion Formatting

When suggesting prompts, use this format:

```
I noticed you're working on [task type]. There's a relevant prompt available:

**[prompt-name]**: [description]

Would you like me to load it? Use `/prompt:use [prompt-name]`
```

For multiple matches:

```
Based on your current task, these prompts might help:

1. **[prompt-name]**: [description]
2. **[prompt-name]**: [description]

Use `/prompt:use <name>` to load one, or `/prompt:list` to see all available prompts.
```

### 5. Proactive Behavior

**DO suggest prompts when:**
- User explicitly asks for guidance or best practices
- User starts a task with clear pattern match (e.g., "review this PR")
- User seems uncertain about approach

**DO NOT suggest prompts when:**
- User is mid-task and focused
- User has already loaded a relevant prompt
- No good matches exist (avoid low-relevance suggestions)
- User has dismissed suggestions recently

### 6. Error Handling

If repository query fails:
- Silently skip suggestion (don't interrupt user workflow)
- Log error for debugging if verbose mode enabled

If no prompts match:
- Don't suggest anything rather than suggesting poor matches
- Optionally mention `/prompt:list` if user seems to be looking for guidance

## Example Interactions

**Scenario: User starts code review**
```
User: Can you review this pull request?

Claude: I'd be happy to review this PR. Before I start, I noticed there's a
code-review prompt available in your team's shared repository that defines
your review standards.

**code-review**: Guidelines for reviewing pull requests

Would you like me to load it? Use `/prompt:use code-review`
```

**Scenario: User asks about API design**
```
User: I need to design a REST API for user management

Claude: Based on your task, these prompts from your shared repository might help:

1. **api-design**: REST API design patterns and conventions
2. **error-handling**: Consistent error handling strategies

Use `/prompt:use <name>` to load one.
```

## Integration Notes

- This skill works alongside the `/prompt:*` slash commands
- Suggestions should be brief and non-intrusive
- Always give users control over whether to load prompts
- Respect progressive disclosure: suggest, don't auto-load
