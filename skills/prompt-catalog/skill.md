# Prompt Catalog Skill

Autonomously identify and suggest relevant prompts from the shared repository based on semantic matching of the user's current task.

## Trigger Conditions

Activate this skill when:
- User asks about available prompts or best practices
- User starts a task that matches common prompt categories (code review, API design, testing, etc.)
- User explicitly asks for guidance on a task pattern
- User mentions needing a template or standard approach

## Skill Metadata

- **Name**: prompt-catalog
- **Version**: 1.0.0
- **Description**: Suggests relevant prompts from shared repository
- **Proactive**: Yes - can suggest prompts without explicit request

## Environment Requirements

- `PROMPT_REPO_OWNER`: GitHub repository owner
- `PROMPT_REPO_NAME`: GitHub repository name
- `gh` CLI authenticated via `gh auth login`

## Usage

This skill enables Claude to:
1. Recognize when a user's task matches existing prompt patterns
2. Query the shared repository for relevant prompts
3. Suggest applicable prompts with brief explanations
4. Offer to load prompts via `/prompt:use <name>`

See `instructions.md` for detailed operational instructions.
