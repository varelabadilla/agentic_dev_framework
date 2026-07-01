# CLAUDE.md — {PROJECT_NAME}

This file contains instructions for Claude Code when working on this project.
Read this file in full before making any changes to the codebase.

---

## What is {PROJECT_NAME}

{PROJECT_DESCRIPTION}

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | {e.g. Node.js 20+} |
| Framework | {e.g. NestJS + TypeScript} |
| ORM | {e.g. Prisma} |
| Database | {e.g. PostgreSQL} |
| API | {e.g. REST} |
| Auth | {e.g. RS256 JWT + httpOnly cookie} |

---

## Development Server

- **Port:** {PORT}
- **Start command:** `npm run start:dev`
- **Swagger:** `http://localhost:{PORT}/api/docs`

---

## Global Code Rules

- All code, comments, and documentation in **English**
- **TypeScript always** — never plain JavaScript
- Conventional commits: `feat:`, `fix:`, `chore:`, `refactor:`, `docs:`
- Never hardcode secrets — always use environment variables
- Never store derived values in the database
- Prisma transactions for multi-step operations

---

## Architecture Rules

{Describe the key architectural constraints for this project. Examples:}
- This service is independent — no imports or coupling to {other system}
- RS256 asymmetric signing only — never HS256
- Access tokens: short-lived ({expiry}), in-memory on client
- Refresh tokens: long-lived ({expiry}), httpOnly cookie, server-side revocation

---

## Role Hierarchy

{Describe the roles in this project, or remove this section if not applicable.}

```
{role 1}  → {capabilities}
{role 2}  → {capabilities}
{role 3}  → {capabilities}
```

---

## Schema Overview

{Paste a concise summary of your Prisma schema here. Update when schema changes.}

```
{Entity}   id, field1, field2, isActive
```

---

## JWT Payload Structure

{Describe your JWT payload, or remove this section if not applicable.}

```json
{
  "sub": "user-uuid",
  "email": "user@example.com",
  "role": "member"
}
```

---

## Key Endpoints

{List your main endpoints. Update as endpoints are added.}

Public:
```
POST /{path}
GET  /{path}
```

Protected:
```
POST /{path}
GET  /{path}
```

---

## Environment Variables

```
DATABASE_URL
JWT_PRIVATE_KEY
JWT_PUBLIC_KEY
JWT_ACCESS_EXPIRY
JWT_REFRESH_EXPIRY
PORT                   (default: {PORT})
```

---

## Directory Structure

```
src/
├── {module}/          # {description}
├── {module}/          # {description}
└── common/            # Guards, decorators, interceptors, filters
```

---

## Standards and Rules

Detailed standards are maintained as portable rule files in `.claude/rules/`.
Read the relevant file before working on that area.

| File | Covers |
|---|---|
| `.claude/rules/code-standards-nodejs-typescript.md` | Language, NestJS patterns, API design |
| `.claude/rules/git-conventions.md` | Commit format, staging rules |
| `.claude/rules/database-conventions-postgresql.md` | Naming, datetime, data integrity |
| `.claude/rules/orm-conventions-prisma.md` | Schema mapping, transactions, queries |
| `.claude/rules/dev-environment.md` | Server lifecycle, temp files |
| `.claude/rules/dev-security-standards.md` | Secrets, hashing, cryptography |
| `.claude/rules/phase-workflow.md` | Phase cycle, task states, definition of done |
| `.claude/rules/documentation-standards.md` | Doc updates, decision log, contract boundaries |
| `.claude/rules/working-mode.md` | Claude Code operating principles |
| `.claude/rules/diagram-standards.md` | When and how to create diagrams |
| `.claude/rules/project-security-standards.md` | Project-specific security rules |
| `.claude/rules/project-tech-standards-nodejs.md` | Project-specific tech conventions |

---

## Development Workflow — Slash Commands

### Initialization (run once)
```
/init-project          → set up project structure and files
/define                → conversational definition session across 8 blocks
/define-adjust         → refine the definition draft
/define-approve        → lock the draft for generation
/define-generate       → generate all docs/ files, update CLAUDE.md and README.md
```

### Phase cycle (repeat per phase)
```
/plan phase-N          → generate implementation plan, records session start
/adjust {text or path} → refine the plan
/approve               → confirm the plan
/execute phase-N       → implement the approved plan
/docs phase-N          → update project documentation
/clean-up              → clear .runbook/, records session end
```

### Utility commands
```
/proposal {idea}                          → create a structured proposal
/proposal-to-phase {path}                 → convert proposal to phase draft
/proposal-approve                         → write the approved phase files
/plan-discard                             → discard the current plan
/time-report                              → show time tracking summary
/update-structure                         → scan repo and write STRUCTURE.md
```

All commands communicate via `.runbook/` (gitignored). `activity.log` is permanent and never deleted.

---

## Temporary Files

Use `.claude/tmp/` as the scratchpad for all intermediate files. This directory is git-ignored.
Never use system temp directories.

---

_This file is the authoritative reference for Claude Code on this project.
Update it whenever architectural decisions change._
