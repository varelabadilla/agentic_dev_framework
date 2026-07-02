# templates/

Template files consumed by `/init-project` when setting up a new project from this framework.

**`bootstrap/` (this folder's parent) exists only in the framework repository.** After running `/init-project` in a forked repo, the contents of `bootstrap/` — including these templates — are distributed to their final destinations and `bootstrap/` is deleted.

---

## Files

| File | Destination after init | Purpose |
|---|---|---|
| `CLAUDE-template.md` | `{project root}/CLAUDE.md` | Base template for project-specific Claude Code instructions |
| `decision-ai-log-template.md` | `{project root}/decision-ai-log.md` | Empty log for planning session decisions |
| `phase-template.md` | `docs/phases/phase-template.md` | Structure reference for new phase files |
| `project-setup-template.md` | `{project root}/docs/planner/project-setup.md` | Template for the Claude.ai Project Setup — configure and upload |
| `project-security-standards-template.md` | `.claude/rules/project-security-standards.md` | Project-specific security rules (extends `dev-security-standards.md`) |
| `project-tech-standards-nodejs-template.md` | `.claude/rules/project-tech-standards-nodejs.md` | Project-specific Node.js tech conventions |

---

## After Initialization

Once `/init-project` completes, these files require manual completion:

1. **`CLAUDE.md`** — fill in Architecture Rules, Schema Overview, Key Endpoints, and Environment Variables for your project
2. **`.claude/rules/project-security-standards.md`** — add project-specific security rules
3. **`.claude/rules/project-tech-standards-nodejs.md`** — add project-specific Prisma conventions, business rules, and what-not-to-do list
4. **`docs/planner/project-setup.md`** — complete all placeholders and upload to your Claude.ai Project as the project instructions

The `phase-template.md` in `docs/phases/` is a reference — copy it when creating new phase files.
