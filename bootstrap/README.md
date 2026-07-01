# bootstrap/

Template files consumed by `/init-project` when setting up a new project from this framework.

**This folder exists only in the framework repository.** After running `/init-project` in a forked repo, the contents of `bootstrap/` are distributed to their final destinations and this folder is deleted.

---

## Files

| File | Destination after init | Purpose |
|---|---|---|
| `CLAUDE.md` | `{project root}/CLAUDE.md` | Base template for project-specific Claude Code instructions |
| `decision_ai_log.md` | `{project root}/decision_ai_log.md` | Empty log for planning session decisions |
| `phase-template.md` | `docs/phases/phase-template.md` | Structure reference for new phase files |
| `project-setup-template.md` | `{project root}/project-setup-template.md` | Template for the Claude.ai Project Setup — configure and upload |
| `project-security-standards.md` | `.claude/rules/project-security-standards.md` | Project-specific security rules (extends `dev-security-standards.md`) |
| `project-tech-standards-nodejs.md` | `.claude/rules/project-tech-standards-nodejs.md` | Project-specific Node.js tech conventions |

---

## After Initialization

Once `/init-project` completes, these files require manual completion:

1. **`CLAUDE.md`** — fill in Architecture Rules, Schema Overview, Key Endpoints, and Environment Variables for your project
2. **`.claude/rules/project-security-standards.md`** — add project-specific security rules
3. **`.claude/rules/project-tech-standards-nodejs.md`** — add project-specific Prisma conventions, business rules, and what-not-to-do list
4. **`project-setup-template.md`** — complete all placeholders and upload to your Claude.ai Project as the project instructions

The `phase-template.md` in `docs/phases/` is a reference — copy it when creating new phase files.
