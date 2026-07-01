# rules/

Portable rule files for Claude Code. Copy the entire `rules/` folder to `.claude/rules/` in any new project.

After copying, `CLAUDE.md` should reference these files so Claude Code knows to read them before working on a specific area.

---

## Files

| File | Applies to | Portable to |
|---|---|---|
| `code-standards-nodejs-typescript.md` | Language rules, NestJS patterns, API design | Any Node.js + TypeScript project |
| `git-conventions.md` | Commit format, staging, branch rules | Any Git project |
| `database-conventions-postgresql.md` | Table naming, datetime storage, data integrity | Any PostgreSQL project |
| `orm-conventions-prisma.md` | Schema mapping, client import, transactions | Any Prisma project |
| `dev-environment.md` | Server lifecycle, port management, temp files | Any local dev project |
| `dev-security-standards.md` | Secrets, hashing, cryptography, input validation | Any project handling auth or sensitive data |
| `phase-workflow.md` | Phase cycle, task states, definition of done | Any project using this framework |
| `documentation-standards.md` | Doc update rules, decision log, contract boundaries | Any project using this framework |
| `working-mode.md` | Claude Code operating principles and approval gates | Any project using this framework |
| `diagram-standards.md` | When and how to create flow diagrams | Any project with documentation diagrams |

---

## Project-Specific Files

After running `/init-project`, two additional files are created in `.claude/rules/` from `bootstrap/` templates:

| File | Purpose |
|---|---|
| `project-security-standards.md` | Security rules specific to this project (extends `dev-security-standards.md`) |
| `project-tech-standards-nodejs.md` | Technical conventions specific to this project and stack |

These files are not in this `rules/` folder — they live only in your project after initialization.
