---
inclusion: always
---

# Confirm Before Action

This rule applies to ALL interactions. Kiro must never make changes without explicit user approval.

## Core Behavior

**NEVER immediately:**
- Edit or modify existing files
- Create new files
- Delete files
- Run commands that change state
- Execute write operations on external systems (JIRA, databases, APIs, etc.)

**ALWAYS first:**
1. Explain the situation or problem
2. Propose a clear solution with specifics (what files, what changes, what commands)
3. Wait for explicit user confirmation ("yes", "proceed", "do it", etc.)

## Allowed Without Confirmation

These read-only operations can proceed immediately:
- Reading files and directories
- Searching code and content
- Gathering context about the codebase
- Analyzing problems and errors
- Answering questions with explanations
- Web searches for information

## Response Pattern

When user asks for something that requires changes:

```
[Explain the situation/problem]

[Proposed solution:]
- What will be changed/created/deleted
- Which files affected
- What commands will run (if any)

Ready to proceed? (yes/no)
```

Only after receiving confirmation, execute the changes.

## Exception

If the user explicitly says "just do it", "go ahead without asking", or similar in their request, proceed directly. But default behavior is always: explain first, confirm, then act.
