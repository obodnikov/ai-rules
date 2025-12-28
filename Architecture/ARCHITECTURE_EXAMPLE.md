# Architecture Overview

## 1. Purpose of This Document

## 2. High-Level System Overview

## 3. Repository Structure
- /frontend
- /backend
- /infra
- /scripts
- /docs

## 4. Core Components
### 4.1 Frontend
### 4.2 Backend
### 4.3 Automation / Jobs
### 4.4 External Integrations

## 5. Data Flow & Runtime Model

## 6. Configuration & Environment Assumptions

## 7. Stability Zones
- Stable
- Semi-stable
- Experimental

## 8. AI Coding Rules and Behavioral Contracts

AI-assisted development in this project is governed by dedicated rule files.

**This document (ARCHITECTURE.md) does NOT redefine coding rules.**

Instead, all AI coders MUST:

- Locate and read all relevant `AI*.md` files before making any changes
- Apply the rules from those files strictly and consistently
- Resolve conflicts between AI rules conservatively or escalate as an open question

### Authoritative AI Rule Files

Depending on the stack and scope of changes, AI must follow:

- `AI.md` — global, cross-project AI behavior rules
- `AI_FRONTEND.md` — frontend-specific rules (if present)
- `AI_BACKEND.md` — backend-specific rules (if present)
- `AI_INFRA.md` — infrastructure / Docker / CI rules (if present)

### Rule Precedence (Highest → Lowest)

1. Explicit instructions from the user in the current task
2. Stack-specific `AI_*.md`
3. Global `AI.md`
4. This ARCHITECTURE.md (architecture constraints only)
5. Implicit conventions inferred from codebase

If any rule conflicts or ambiguity is detected, the AI must stop and ask for clarification.

