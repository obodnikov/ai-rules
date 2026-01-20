---
inclusion: always
---

# JIRA Safety Rules

These rules govern all interactions with JIRA through the MCP tools to prevent accidental or unintended changes.

## Core Principle

**ALL write operations to JIRA require:**
1. A clear implementation plan explaining what will be changed and why
2. Explicit user confirmation before execution

Never proceed with any JIRA modification without both steps completed.

## Blocked Operations (Never Execute)

The following operations are **strictly forbidden** regardless of user request:

### Initiatives
- **DO NOT** create new Initiatives
- **DO NOT** update any field on Initiatives (summary, description, labels, components, assignee, etc.)
- **DO NOT** transition Initiatives to any status
- **DO NOT** add comments to Initiatives that imply status changes

### Epics
- **DO NOT** transition Epics to "Close", "Done", "Resolved", or any terminal/completed status
- If asked to close an Epic, explain this is blocked and suggest the user do it manually in JIRA

## Allowed Operations (With Confirmation)

These operations are permitted but require implementation plan + explicit confirmation:

### Read Operations (No confirmation needed)
- `get_issue` / `get_issue_full` - Reading issue details
- `search_issues` / `get_all_issues` / `get_issues_page` - Searching issues
- `get_project` / `list_projects` - Reading project info
- `get_issue_transitions` - Viewing available transitions

### Write Operations (Require plan + confirmation)
- `create_issue` - Creating new issues (except Initiatives)
- `update_issue` - Updating issue fields
- `add_comment` - Adding comments
- `transition_issue` - Changing issue status (except blocked cases above)

## Confirmation Protocol

Before any write operation, present:

```
## JIRA Change Plan

**Action:** [What will be done]
**Target:** [Issue key or "New issue"]
**Project:** [Project key]
**Issue Type:** [Type if applicable]
**Changes:**
- [Field]: [Current value] → [New value]

Do you confirm this change? (yes/no)
```

Wait for explicit "yes" or equivalent confirmation before proceeding.

## Error Handling

If a blocked operation is requested:
1. Clearly state the operation is blocked by safety rules
2. Explain why (Initiative protection or Epic closure restriction)
3. Suggest the user perform the action manually in JIRA if needed
