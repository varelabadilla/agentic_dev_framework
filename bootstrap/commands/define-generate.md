# /define-generate — Generate Project Documentation from Approved Definition

## Usage

`/define-generate`

Run this after `/define-approve`. This command reads the approved definition
draft and generates all project documentation files, then updates `CLAUDE.md`
and `README.md` with the full project context.

---

## Process

1. Check that `.claude/tmp/project-definition-draft.md` exists.
   If it does not, inform the user:
   "No definition draft found. Run /define first, then /define-approve."
   Stop.

2. Read `.claude/tmp/project-definition-draft.md` in full.

3. Verify the Status line reads:
   `Status: Approved — ready for /define-generate`
   If it does not, inform the user:
   "The definition draft has not been approved. Run /define-approve first."
   Stop.

4. Read `CLAUDE.md` and `README.md` in full before modifying them.

5. Generate the following files in order.
   For each file: if it already exists, read it in full before overwriting.

---

### File 1 — `docs/product_definition.md`

Derived from: Block 1 (Product Identity), Block 2 (Actors), Block 7 (What It Is NOT)

```markdown
# Product Definition — {PROJECT_NAME}

> Version 0.1 | {date} | Solo developer

---

## 1. What is {PROJECT_NAME}

{Expanded summary from Block 1}

---

## 2. Problem it Solves

{Problem statement from Block 1, expanded into 2-3 sentences}

---

## 3. Who Uses It

| Actor | Description |
|---|---|
{Rows derived from Block 2 actors}

---

## 4. Core Concepts

{Key concepts derived from Block 3 entities and Block 2 actors}

---

## 5. Key Design Principles

{Derived from Block 6 restrictions and Block 1 boundaries}

---

## 6. What {PROJECT_NAME} is NOT

{List derived from Block 7}

---

## 7. Roadmap

{Phase sequence derived from Block 8, grouped as Phase 1 / Phase 2 / Future}

---

*This document is a living reference. Update it when product scope or priorities change.*
```

---

### File 2 — `docs/information_architecture.md`

Derived from: Block 3 (Core Entities), Block 2 (Actors), Block 4 (Key Flows)

```markdown
# Information Architecture — {PROJECT_NAME}

> Version 0.1 | {date}

---

## 1. Overview

{Summary of entity hierarchy derived from Block 3}

---

## 2. Entity Hierarchy

{ASCII or text diagram of entity relationships}

---

## 3. Entities

{One section per entity with field table derived from Block 3}

---

## 4. Role Definitions

{Table derived from Block 2 actor hierarchy}

---

*This document is a living reference. Update it when schema or access logic changes.*
```

---

### File 3 — `docs/user_flows.md`

Derived from: Block 4 (Key Flows), Block 2 (Actors)

```markdown
# User Flows — {PROJECT_NAME}

> Version 0.1 | {date}

---

## 1. Overview

{Brief description of the flows documented here}

{One section per flow identified in Block 4, each containing:}
## {N}. {Flow Name}

{Step-by-step description}

**Rules:**
{Business rules and constraints for this flow}

---

*This document is a living reference. Update it when flows change or new flows are added.*
```

---

### File 4 — `docs/technical_decisions.md`

Derived from: Block 5 (Tech Stack), Block 6 (Restrictions and Risks)

```markdown
# Technical Decisions — {PROJECT_NAME}

> Version 0.1 | {date}

---

## 1. Introduction

This document records all technical decisions made for {PROJECT_NAME}.

---

## 2. Tech Stack Summary

| Layer | Technology | Notes |
|---|---|---|
{Rows derived from Block 5}

---

## 3. Individual Decisions

{One subsection per stack choice from Block 5, with rationale from the conversation}

---

## 4. Pending Decisions

| Decision | Notes |
|---|---|
{Items from Block 6 risks that are unresolved or deferred}

---

*This document is a living draft. Update it as technical decisions evolve.*
```

---

### File 5 — `docs/development_plan.md`

Derived from: Block 8 (Initial Phases), Block 5 (Tech Stack)

```markdown
# Development Plan — {PROJECT_NAME}

> Version 0.1 | {date} | Solo developer

---

## 1. Introduction

{Brief description of the project and how this plan is organized}

---

## 2. Phase Overview

| Phase | Name | Goal | Status |
|---|---|---|---|
{Rows derived from Block 8 proposed phase sequence}

---

## 3. Phase Detail Files

Full task detail for each phase lives in its own file under `docs/phases/`.

{Links — initially empty, populated as phases are planned}

---

*This document is a living draft. Tasks may be added, reordered, or split as development progresses.*
```

---

### File 6 — `docs/decision_log.md`

Structure only — no entries yet.

```markdown
# Decision Log — {PROJECT_NAME}

> Append-only. Never modify or renumber existing entries.
> New entries always go at the end.

---

<!-- First entry will be added by /plan-docs phase-N after Phase 1 completes -->
```

---

### File 7 — `docs/dev_status.md`

Initial state.

```markdown
# Development Status — {PROJECT_NAME}

> Last updated: {date}

---

## Current Phase

Phase 0 — Project initialized. No phases started yet.

---

## Phase Completion Status

| Phase | Name | Status |
|---|---|---|
{Rows derived from development_plan.md — all ⏳ Pending}

---

## Known Issues

None.

---

*Updated by /plan-docs phase-N after each phase completes.*
```

---

### File 8 — Update `CLAUDE.md`

Fill in the sections that were left as placeholders after `/init-project`:
- Architecture Rules — derived from Block 5 and Block 6
- Role Hierarchy — derived from Block 2
- Schema Overview — derived from Block 3
- Key Endpoints — derived from Block 4 (if API endpoints are identified)
- Environment Variables — derived from Block 5 and Block 6

Do not overwrite sections the user has already completed manually.
Read the current `CLAUDE.md` in full and update only the placeholder sections.

---

### File 9 — Update `README.md`

Fill in or expand:
- Project description — from Block 1
- Features list — from Block 2 and Block 4
- Tech stack table — from Block 5
- Getting started section — from Block 5

Read the current `README.md` in full before modifying.

---

6. Verify all generated files:
   - No broken markdown tables
   - No unclosed code blocks
   - No remaining `{placeholder}` values that should have been filled
   - No duplicate section headings

7. Stage all generated and modified files:
   ```bash
   git add docs/ CLAUDE.md README.md
   ```

8. Append to `activity.log`:
   `[{timestamp}] /define-generate: Project documentation generated. {N} files created/updated.`

9. Inform the user:
   "Project documentation generated.

   ## Files created
   - docs/product_definition.md
   - docs/information_architecture.md
   - docs/user_flows.md
   - docs/technical_decisions.md
   - docs/development_plan.md
   - docs/decision_log.md
   - docs/dev_status.md

   ## Files updated
   - CLAUDE.md
   - README.md

   ## Next steps
   - Review the generated files and fill in any remaining gaps
   - If you want to enrich the documentation with Claude.ai, upload the docs/ files
     to your Claude.ai project and refine them there
   - When ready to start development, run: /plan phase-1

   Commit message:
   `docs: initialize project documentation from definition session`"

---

## Restrictions

- Do not generate source code files
- Do not run database commands
- Do not run `git commit` — stage only
- All content in English
