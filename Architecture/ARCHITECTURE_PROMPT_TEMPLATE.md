# Architecture Documentation Prompt Template

**Purpose:** Reusable prompt for creating ARCHITECTURE.md in any project

---

## Quick Copy-Paste Prompt

```
You are acting as a senior software architect and AI coding assistant.

Context:
This is a [DESCRIBE: long-living/new, personal/commercial, monolith/microservices] [TYPE: web app/CLI/library/mobile app] project that has evolved over time.
Coding rules and AI behavior are defined in dedicated AI*.md files
and MUST NOT be duplicated or redefined in architecture documents.

Your task is to analyze the repository structure, existing documentation in root and docs/ directories, and previous conversations under docs/chats/ directory (if exists),
then create or update ARCHITECTURE.md as an architectural source of truth.

––––––––––––––––––––
IMPORTANT CONSTRAINT:

- Do NOT redefine coding rules, formatting rules, or stack-specific practices
- Do NOT duplicate content from AI*.md files
- ARCHITECTURE.md must only reference AI*.md files as authoritative sources
- If AI rules are missing for a stack, explicitly mark this as a gap
- Target length: 250-300 lines maximum
- Use tmp/ARCHITECT.md as the pattern template (if available)

––––––––––––––––––––
YOUR OBJECTIVES:

1. Analyze repository structure and identify architectural components
2. Cross-check existing documentation and identify outdated or conflicting parts
3. Check previous conversations in docs/chats/ for context
4. Document the current architecture as it exists today (not as imagined)
5. Map components to stability zones:
   - ✅ Stable (production-ready, low risk to change)
   - 🔄 Semi-Stable (functional, may evolve)
   - ⚠️ Experimental (working, may be replaced)
   - 🔮 Planned (not yet implemented)
6. Add a clear section that instructs AI coders to follow AI*.md files
7. Create rule precedence hierarchy for conflict resolution
8. Use ASCII diagrams for data flows (not prose)
9. Link to detailed docs, don't duplicate them

––––––––––––––––––––
REQUIRED STRUCTURE (9 sections):

1. Purpose of This Document (5-10 lines)
2. High-Level System Overview (20-30 lines with ASCII diagram)
3. Repository Structure (30-50 lines, actual directory tree)
4. Core Components (40-60 lines, 4.1-4.4 subsections)
5. Data Flow & Runtime Model (30-50 lines, ASCII diagrams)
6. Configuration & Environment Assumptions (20-30 lines)
7. Stability Zones (30-40 lines, mapped to real components)
8. AI Coding Rules and Behavioral Contracts (40-60 lines, CRITICAL)
9. Quick Start for AI Assistants (20-30 lines)

––––––––––––––––––––
FINAL GOAL:

ARCHITECTURE.md should allow any AI coder to:
- Understand system structure in <5 minutes
- Know WHERE to find coding rules (Section 8)
- Avoid architectural violations (Section 7)
- Behave conservatively and predictably
- Know what's stable vs experimental

VALIDATION:
- Total length: 250-300 lines ✓
- No coding rule duplication ✓
- Specific to this project (not generic) ✓
- Section 8 has rule precedence hierarchy ✓
- Uses diagrams over prose ✓
- Links to docs, doesn't duplicate ✓
```

---

## Customization Guide

Replace the placeholders in the prompt above:

### [DESCRIBE]
- `long-living personal PET project`
- `new commercial startup`
- `enterprise monolith application`
- `experimental microservices platform`

### [TYPE]
- `web application` (full-stack)
- `CLI tool`
- `library/SDK`
- `mobile app` (iOS/Android/cross-platform)
- `API service`
- `data pipeline`

### Examples

**For a web app:**
```
This is a long-living personal web application project that has evolved over time.
```

**For a CLI tool:**
```
This is a new open-source CLI tool project.
```

**For a microservices platform:**
```
This is a commercial microservices platform that has been in production for 2 years.
```

---

## Pre-Prompt Checklist

Before running the prompt, ensure:

- [ ] AI*.md files exist (or note they're missing)
- [ ] README.md exists (for user-facing docs)
- [ ] Basic directory structure is in place
- [ ] You have tmp/ARCHITECT.md available as pattern reference
- [ ] You know the project type (web app, CLI, library, etc.)

---

## Post-Generation Validation

After AI generates ARCHITECTURE.md, check:

### Structure
- [ ] Has all 9 sections
- [ ] Under 300 lines total
- [ ] Uses numbered sections
- [ ] Clear headings and subsections

### Content
- [ ] Specific to YOUR project (not generic)
- [ ] Documents current state (not ideal/planned)
- [ ] No coding rule duplication
- [ ] ASCII diagrams for data flows
- [ ] Links to other docs (not duplicates)

### Section 8 (Critical)
- [ ] States "does NOT define coding rules"
- [ ] Lists all AI*.md files
- [ ] Has rule precedence hierarchy
- [ ] Includes "stop and ask" for conflicts

### Quality
- [ ] AI can understand system structure quickly
- [ ] Components mapped to stability zones
- [ ] Key architectural decisions documented
- [ ] Quick start guide exists

---

## Iterative Refinement

If the first generation is too long or too vague:

### Iteration 1: "Make it more concise"
```
The ARCHITECTURE.md is [TOO LONG: 400 lines / TOO VAGUE: uses generic examples].

Please refine it:
- Target 250-300 lines maximum
- Use actual module names (not "Module X")
- Replace prose with diagrams where possible
- Link to docs instead of duplicating content
- Focus on decisions, not implementation details
```

### Iteration 2: "Improve Section X"
```
Section [NUMBER] needs improvement:
- [ISSUE 1: e.g., too generic, missing details, duplicates coding rules]
- [ISSUE 2: ...]

Please revise Section [NUMBER] only, keeping the same line budget.
```

### Iteration 3: "Add missing stability zones"
```
Section 7 (Stability Zones) needs to map these actual components:
- [COMPONENT 1] - [SUGGESTED ZONE]
- [COMPONENT 2] - [SUGGESTED ZONE]

Please add these to Section 7 with brief rationale.
```

---

## Variation: When AI Rules Don't Exist Yet

If your project doesn't have AI*.md files:

```
MODIFIED PROMPT - No Existing AI Rules:

[... same intro ...]

IMPORTANT NOTES:
- This project does NOT yet have AI*.md files
- ARCHITECTURE.md Section 8 should:
  1. Note the ABSENCE of AI rules explicitly
  2. Mark this as a gap to be filled
  3. Suggest which AI_*.md files should be created based on the stack
  4. Provide temporary guidance until AI rules are created
- Keep total length under 300 lines

Example Section 8 output:
```markdown
## 8. AI Coding Rules and Behavioral Contracts

⚠️ **CRITICAL: This project does NOT yet have AI*.md rule files.**

### Missing AI Rule Files (To Be Created)

Based on the tech stack, the following AI rule files SHOULD be created:

**Backend:**
- `AI_PYTHON.md` - Python coding standards (PEP8, type hints, async patterns)
- `AI_FASTAPI.md` - FastAPI-specific patterns and best practices

**Frontend:**
- `AI_REACT.md` - React patterns, hooks, component structure

**General:**
- `CLAUDE.md` - Project workflow (propose before implementing, git rules)

### Temporary Guidance (Until AI Rules Exist)

Until proper AI rule files are created, follow these principles:
- Propose changes before implementing
- Match existing code style in the repository
- Write tests for new functionality
- Ask for clarification on ambiguous requirements

### Rule Precedence (Once AI Rules Exist)
1. Explicit user instructions
2. Stack-specific AI_*.md files
3. This ARCHITECTURE.md (architecture constraints)
```
```

---

## Example Usage Workflow

### Step 1: Prepare
```bash
# Navigate to project root
cd /path/to/your/project

# Check for existing docs
ls -la *.md
ls -la docs/

# Check for AI rules
ls -la AI*.md
```

### Step 2: Customize Prompt
```
Replace placeholders:
- [DESCRIBE] → "long-living commercial web application"
- [TYPE] → "web application"
```

### Step 3: Run Prompt
```
Paste customized prompt to Claude/AI assistant
Wait for generation
```

### Step 4: Validate
```
Check generated ARCHITECTURE.md:
- Line count: wc -l ARCHITECTURE.md
- Has 9 sections: grep "^## [0-9]" ARCHITECTURE.md
- No rule duplication: grep -i "coding rule\|code style" ARCHITECTURE.md
```

### Step 5: Refine (if needed)
```
If issues found, use iterative refinement prompts above
```

### Step 6: Commit
```bash
git add ARCHITECTURE.md
git commit -m "docs: add architectural documentation"
```

---

## Integration with CLAUDE.md

If your project has CLAUDE.md, add this section:

```markdown
## Architecture Documentation

This repository uses ARCHITECTURE.md as the architectural source of truth.

**Before making changes, read:**
1. ARCHITECTURE.md (system structure and stability zones)
2. Relevant AI*.md files (coding rules for your stack)
3. docs/chats/ (previous implementation conversations)

**ARCHITECTURE.md tells you:**
- What components exist and how they connect
- What's stable (don't break) vs experimental (may change)
- Where to find coding rules (AI*.md files)
- Key architectural decisions and their rationale

**AI*.md files tell you:**
- How to write code for each stack
- Formatting and style rules
- Best practices and patterns

Never duplicate coding rules between ARCHITECTURE.md and AI*.md files.
```

---

## Maintenance Prompt

Use this prompt to update ARCHITECTURE.md when the project evolves:

```
The project architecture has changed:

[DESCRIBE CHANGES: e.g., "Added new microservice for payments", "Migrated database from SQLite to PostgreSQL", "Refactored frontend to use Next.js"]

Please update ARCHITECTURE.md to reflect these changes:

1. Update Section [NUMBER] to document [SPECIFIC CHANGE]
2. Update Section 7 (Stability Zones) if component stability changed
3. Add architectural decision rationale if this was a major change
4. Keep total length under 300 lines
5. Update "Last Updated" date at the top

Preserve all other sections unchanged.
```

---

## Success Checklist

Your ARCHITECTURE.md is successful when:

✅ **Onboarding:** New AI assistant can start contributing in <5 minutes
✅ **Clarity:** System structure is immediately understandable
✅ **Safety:** Stability zones prevent accidental breakage
✅ **Rules:** No confusion about where coding rules live
✅ **Brevity:** Under 300 lines, stays current
✅ **Decisions:** Key architectural choices are documented with rationale
✅ **Precedence:** Rule conflicts have clear resolution path

---

## Common Questions

### Q: Should I include API endpoint documentation?
**A:** No. Link to API docs (e.g., OpenAPI/Swagger) instead.

### Q: What about coding standards like "use tabs" or "max line length"?
**A:** No. That belongs in AI*.md files, not ARCHITECTURE.md.

### Q: My project is small (1-2 files). Do I need this?
**A:** No. ARCHITECTURE.md is for projects with multiple components/subsystems.

### Q: Can I exceed 300 lines if my project is huge?
**A:** Resist the urge. If you need more, link to detailed docs in docs/ directory.

### Q: What if my architecture is in flux (early stage)?
**A:** Mark everything as ⚠️ Experimental in Section 7. Update as it stabilizes.

### Q: Should I document third-party integrations?
**A:** Yes, briefly in Section 4.4 (External Integrations). Link to integration docs.

---

**End of Prompt Template**
