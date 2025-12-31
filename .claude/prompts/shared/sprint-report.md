---
description: Generate sprint reports for MD project with Jira integration
tags: [sprint, report, jira, project-management]
created: 2025-12-31
---

# Sprint Report Generation Prompt

Use this prompt to generate sprint reports for the MD project.

---

## Prompt Template

```
Generate a sprint report for MD project Sprint [SPRINT_NUMBER].

Requirements:
1. Search for all issues in project MD for Sprint [SPRINT_NUMBER]
2. Exclude all sub-tasks from the report
3. Group issues by functional area:
   - Agent
   - CDT (Culinary Data Team)
   - Supply Chain
   - Nutrition & Recipe
   - Technical Infrastructure
   - 3P Integration
   - Testing & Quality
4. Format as two-layer bullet points (category, then items)
5. Use concise descriptions without ticket numbers
6. Do not show status (no Done, In Progress, etc.)
7. No emojis
8. Save as MD-2025-Sprint-[SPRINT_NUMBER]-Confluence.md

Example format:
- Agent
    - Search item info via natural language (Through BigQuery MCP)
    - Grant Jira tools the DORA board access
- CDT (Culinary Data Team)
    - Deprecate the Slipped 'Orderable' Field from Cookbook
    - Food Science - Shelf Life + Slacking Times all need days/hours/mins
```

---

## Quick Start Command

Copy and paste this for next sprint:

```
Generate a sprint report for MD project Sprint 24.

Requirements:
1. Search for all issues in project MD for Sprint 24
2. Exclude all sub-tasks from the report
3. Group issues by functional area: Agent, CDT (Culinary Data Team), Supply Chain, Nutrition & Recipe, Technical Infrastructure, 3P Integration, Testing & Quality
4. Format as two-layer bullet points (category, then items)
5. Use concise descriptions without ticket numbers
6. Do not show status (no Done, In Progress, etc.)
7. No emojis
8. Save as MD-2025-Sprint-24-Confluence.md
```

---

## Notes

- Adjust sprint number for each report (Sprint 24, Sprint 25, etc.)
- The AI will automatically query Jira using the atlassian MCP tools
- Review and adjust functional area groupings if needed
- File will be saved in the current workspace directory
