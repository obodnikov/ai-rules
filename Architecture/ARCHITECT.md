# ARCHITECT.md - Architecture Documentation Pattern

**Purpose:** Template for creating ARCHITECTURE.md files in any project

---

## What Is This File?

This is a **meta-pattern** that helps AI assistants create consistent, maintainable ARCHITECTURE.md files for software projects.

**Use this when:**
- Starting a new project that will use AI-assisted development
- A codebase has evolved and needs architectural documentation
- AI assistants need guidance on system structure without duplicating coding rules

**Do NOT use this for:**
- Small scripts or single-file projects
- Projects that already have comprehensive architecture docs
- Pure library projects (use README.md instead)

---

## The Architecture Documentation Philosophy

### Core Principles

1. **Separation of Concerns**
   - ARCHITECTURE.md = System structure, decisions, stability zones
   - AI*.md files = Coding rules, formatting, stack-specific practices
   - README.md = User-facing documentation, setup instructions
   - ROADMAP.md = Development timeline and future plans

2. **Brevity Over Completeness**
   - Target: 250-300 lines maximum
   - Document **decisions**, not implementation
   - Link to details, don't duplicate them
   - Use diagrams over prose

3. **Stability Markers**
   - ✅ Stable = Production-ready, low risk to change
   - 🔄 Semi-Stable = Functional, may evolve
   - ⚠️ Experimental = Working, may be replaced
   - 🔮 Planned = Not yet implemented

4. **AI-First Design**
   - Written for AI assistants as primary audience
   - Clear rule precedence for conflict resolution
   - Explicit "stop and ask" guidance
   - Quick start section for common tasks

---

## Standard ARCHITECTURE.md Structure

### Section 1: Purpose of This Document

**What to include:**
- What this document does (structure, decisions, stability)
- What this document does NOT do (no coding rules duplication)
- Who the audience is (AI assistants, new developers)

**Keep it:** 5-10 lines

### Section 2: High-Level System Overview

**What to include:**
- Project type and characteristics (web app, CLI, library, etc.)
- Tech stack summary (languages, frameworks, databases)
- Architecture pattern in ASCII diagram
- Key architectural characteristics (async, microservices, monolith, etc.)

**Keep it:** 20-30 lines with diagram

### Section 3: Repository Structure

**What to include:**
- Directory tree (actual structure, not generic)
- Purpose of each major directory
- Critical paths (entry points, config files, tests)
- What lives where and why

**Keep it:** 30-50 lines

**Format:**
```
project-name/
├── backend/           # Brief purpose
│   ├── app/          # Core application
│   └── tests/        # Test suite
├── frontend/          # Brief purpose
└── docs/             # Documentation
```

### Section 4: Core Components

**What to include:**
- Major subsystems (Frontend, Backend, Jobs, External Integrations)
- Component boundaries and responsibilities
- Key technologies per component
- Integration points between components

**Keep it:** 40-60 lines

**Format:** Use subsections (4.1, 4.2, 4.3, 4.4)

### Section 5: Data Flow & Runtime Model

**What to include:**
- Critical data flows (authentication, main business logic)
- Request/response cycles
- State management patterns
- Configuration loading hierarchy

**Use:** ASCII diagrams, not prose

**Keep it:** 30-50 lines

### Section 6: Configuration & Environment Assumptions

**What to include:**
- Environment variable requirements
- Configuration file structure
- Deployment assumptions (Docker, cloud, local)
- Critical secrets and how they're managed

**Keep it:** 20-30 lines

### Section 7: Stability Zones

**What to include:**
- Map of actual modules/components to stability levels
- Clear guidance on what NOT to change
- Warnings about experimental areas
- Reference to future plans

**Keep it:** 30-40 lines

**Format:**
```
### ✅ Stable (Production-Ready, Low Risk)
- Module X (do not restructure without approval)
- Module Y

### 🔄 Semi-Stable (Functional, May Evolve)
- Module Z (changes require planning)
```

### Section 8: AI Coding Rules and Behavioral Contracts

**What to include:**
- **Critical statement:** "This document does NOT define coding rules"
- List of all AI*.md files and their purposes
- Rule precedence hierarchy (user → stack-specific → global → architecture)
- Conflict resolution process ("stop and ask")
- Key architectural decisions that must NOT be violated

**Keep it:** 40-60 lines

**This is the MOST IMPORTANT section** - it prevents rule duplication and confusion.

### Section 9: Quick Start for AI Assistants

**What to include:**
- Pre-flight checklist before making changes
- Where to look for specific types of information
- Common workflows (new feature, bug fix, architecture question)

**Keep it:** 20-30 lines

---

## Template Prompt for AI Assistants

Use this prompt to generate ARCHITECTURE.md in any project:

```
You are acting as a senior software architect and AI coding assistant.

Context:
This is a [describe project: long-living/new, personal/commercial, etc.] project.
Coding rules and AI behavior are defined in dedicated AI*.md files
and MUST NOT be duplicated or redefined in architecture documents.

Your task is to analyze the repository structure, existing documentation,
and previous conversations, then create ARCHITECTURE.md as an architectural
source of truth.

IMPORTANT CONSTRAINTS:

- Do NOT redefine coding rules, formatting rules, or stack-specific practices
- Do NOT duplicate content from AI*.md files
- ARCHITECTURE.md must only reference AI*.md files as authoritative sources
- If AI rules are missing for a stack, explicitly mark this as a gap
- Target length: 250-300 lines maximum
- Use tmp/ARCHITECT.md as the pattern template

YOUR OBJECTIVES:

1. Analyze repository structure and identify architectural components
2. Cross-check existing documentation (root and docs/ directories)
3. Check previous conversations (docs/chats/ if exists)
4. Document the current architecture as it exists today (not as imagined)
5. Map components to stability zones (Stable, Semi-Stable, Experimental, Planned)
6. Create clear rule precedence hierarchy in Section 8
7. Use ASCII diagrams for data flows, not prose
8. Link to detailed docs, don't duplicate them

FINAL GOAL:

ARCHITECTURE.md should allow any AI coder to:
- Understand system structure quickly
- Know WHERE to find coding rules
- Avoid architectural violations
- Behave conservatively and predictably
- Know what's stable vs experimental

Follow the structure in tmp/ARCHITECT.md exactly.
Keep it under 300 lines.
Use tables and diagrams to save space.
```

---

## Checklist for Quality ARCHITECTURE.md

Use this to validate the generated document:

### Structure
- [ ] Follows 9-section structure exactly
- [ ] Under 300 lines total
- [ ] Each section within target line count
- [ ] Uses numbered sections (1-9)
- [ ] Has table of contents (optional, if helpful)

### Content Quality
- [ ] Documents actual current state (not ideal/planned)
- [ ] Specific to this project (not generic)
- [ ] No coding rule duplication
- [ ] References AI*.md files explicitly
- [ ] Maps real components to stability zones
- [ ] Includes ASCII diagrams for data flows
- [ ] Links to detailed docs instead of duplicating

### Section 8 (Critical)
- [ ] States "does NOT define coding rules" clearly
- [ ] Lists all AI*.md files with purposes
- [ ] Defines rule precedence hierarchy
- [ ] Includes "stop and ask" conflict resolution
- [ ] Lists key architectural decisions to preserve

### Usability for AI
- [ ] AI can find coding rules quickly (Section 8)
- [ ] AI can understand system structure (Sections 2-4)
- [ ] AI knows what's safe to change (Section 7)
- [ ] AI has quick start workflow (Section 9)
- [ ] AI knows where to find details (links throughout)

### Maintainability
- [ ] Short enough to stay current
- [ ] Links to docs that can be updated independently
- [ ] Stability markers help prevent accidental breakage
- [ ] Clear enough for future maintainer to update

---

## Common Mistakes to Avoid

### ❌ Anti-Patterns

1. **Over-Documentation**
   - Don't document every function/class
   - Don't duplicate README.md content
   - Don't include implementation code snippets

2. **Rule Duplication**
   - Don't copy coding standards from AI*.md
   - Don't redefine formatting rules
   - Don't create new coding guidelines

3. **Vagueness**
   - Don't use generic directory names (use actual names)
   - Don't say "various modules" (list them)
   - Don't use placeholder diagrams

4. **Length Creep**
   - Don't exceed 300 lines
   - Don't add "nice to have" sections
   - Don't duplicate content across sections

5. **Staleness Risk**
   - Don't document volatile implementation details
   - Don't include specific version numbers (except in Section 2)
   - Don't copy-paste from other docs that may change

### ✅ Best Practices

1. **Link, Don't Duplicate**
   - Reference implementation.md for deployment
   - Reference configuration.md for config details
   - Reference ROADMAP.md for future plans

2. **Diagram Over Prose**
   - Use ASCII art for data flows
   - Use tables for stability zones
   - Use tree structure for directory layouts

3. **Specificity**
   - Use actual module names
   - Reference real file paths
   - Map to actual stability levels

4. **AI-First Language**
   - Write for AI assistants as primary readers
   - Use imperative language ("Read X before Y")
   - Include explicit "do not" warnings

---

## Example Snippets

### Good Section 2 (High-Level Overview)

```markdown
## 2. High-Level System Overview

E-commerce platform with microservices architecture.

**Key Characteristics:**
- **Type:** Commercial SaaS product
- **Evolution:** 3 years in production
- **Stack:** Node.js + React + PostgreSQL + Redis
- **Pattern:** Event-driven microservices

**Architecture:**
```
API Gateway
    ↓
┌─────────┬─────────┬──────────┐
│ Auth    │ Catalog │ Orders   │
│ Service │ Service │ Service  │
└────┬────┴────┬────┴────┬─────┘
     └─────────┴─────────┘
             ↓
    ┌────────────────┐
    │ PostgreSQL     │
    │ Redis Cache    │
    └────────────────┘
```
```

### Good Section 7 (Stability Zones)

```markdown
## 7. Stability Zones

### ✅ Stable (Production-Ready, Low Risk)
**Do NOT restructure without explicit approval:**
- Authentication service (auth-service/)
- Payment gateway integration (services/payment/)
- Database schema (migrations/)

### 🔄 Semi-Stable (Functional, May Evolve)
**Changes require planning:**
- Recommendation engine (services/recommendations/)
- Search indexing (services/search/)

### ⚠️ Experimental (Working, May Be Replaced)
**Expect changes:**
- AI-powered chatbot (services/chatbot/)
- A/B testing framework (lib/ab-testing/)
```

### Good Section 8 (AI Rules)

```markdown
## 8. AI Coding Rules and Behavioral Contracts

⚠️ **CRITICAL: This document does NOT define coding rules.**

### Authoritative AI Rule Files

**Backend:**
- `AI_NODEJS.md` - Node.js patterns, async/await, error handling
- `AI_API.md` - REST API conventions, versioning
- `AI_DATABASE.md` - PostgreSQL patterns, migrations

**Frontend:**
- `AI_REACT.md` - React patterns, hooks, state management
- `AI_TYPESCRIPT.md` - TypeScript strict mode, type patterns

**Infrastructure:**
- `AI_DOCKER.md` - Container patterns, multi-stage builds

**Project Workflow:**
- `CLAUDE.md` - Propose before implementing, never auto-commit

### Rule Precedence (Highest → Lowest)

1. Explicit user instructions in current task
2. Stack-specific AI_*.md
3. Global AI.md
4. This ARCHITECTURE.md (architecture constraints only)
5. Implicit conventions from codebase

### Conflict Resolution

If rules conflict:
1. STOP - Do not proceed
2. ASK - Present conflict and request clarification
3. DOCUMENT - Update relevant AI_*.md after resolution
```

---

## Adaptation Guide for Different Project Types

### Web Application (Full-Stack)
- Emphasize frontend/backend separation in Section 4
- Include detailed data flow (auth, API calls)
- Map deployment architecture (Docker, cloud)

### CLI Tool / Script
- Simplify Section 4 (fewer components)
- Focus on configuration and environment in Section 6
- Emphasize stability of CLI interface

### Library / SDK
- Section 4: API surface vs internal implementation
- Section 7: Public API (stable) vs internals (may change)
- Add "Breaking Changes Policy" in Section 8

### Microservices
- Section 4: Per-service breakdown
- Section 5: Inter-service communication patterns
- Add service dependency diagram in Section 2

### Mobile App
- Section 4: Platform-specific components (iOS/Android)
- Section 6: Platform assumptions and requirements
- Include app store deployment in Section 6

---

## Maintenance Guidelines

### When to Update ARCHITECTURE.md

**Always update when:**
- Major architectural decision is made (Section 8)
- Component moves between stability zones (Section 7)
- New subsystem is added (Section 4)
- Directory structure changes significantly (Section 3)

**Consider updating when:**
- New AI*.md file is added (Section 8)
- Deployment model changes (Section 6)
- Critical data flow changes (Section 5)

**Do NOT update for:**
- Individual feature additions (unless architectural)
- Bug fixes
- Refactoring within a stable component
- Dependency version bumps

### How to Update

1. Keep changes minimal and focused
2. Update relevant section only
3. Preserve brevity (stay under 300 lines)
4. Update "Last Updated" date at top
5. Consider creating docs/chats/ entry explaining the change

---

## Success Metrics

A good ARCHITECTURE.md succeeds when:

✅ AI assistants can start contributing in <5 minutes reading time
✅ New developers understand system structure quickly
✅ Architectural violations are prevented (stability zones work)
✅ Coding rule confusion is eliminated (clear AI*.md references)
✅ Document stays current (brevity prevents staleness)
✅ Conflicts are resolved consistently (rule precedence works)

---

**End of ARCHITECT.md Pattern**

---

## License

This pattern is released to public domain. Use it in any project, commercial or personal.

**Created by:** AI-assisted software architecture documentation project
**Version:** 1.0
**Last Updated:** 2025-12-28
