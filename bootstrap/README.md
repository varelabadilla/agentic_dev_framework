# bootstrap/

Everything `/init-project` needs to set up a new project from this framework.

**This folder exists only in the framework repository.** After running `/init-project` in a forked repo, its contents are distributed to their final destinations and `bootstrap/` is deleted. See `.claude/commands/init-project.md` for the full copy table.

---

## Contents

| Folder | Copied to | Details |
|---|---|---|
| `rules/` | `.claude/rules/` | [rules/README.md](rules/README.md) |
| `commands/` | `.claude/commands/` | [commands/README.md](commands/README.md) |
| `agents/` | `.claude/agents/` | [agents/README.md](agents/README.md) |
| `templates/` | Varies per file — project root, `.claude/rules/`, `docs/planner/` | [templates/README.md](templates/README.md) |
