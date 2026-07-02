# Git Conventions

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project using Git for version control.

---

## Commit Format

All commits must follow the **Conventional Commits** format:

```
<type>: <short description>
```

### Types

| Type | When to use |
|---|---|
| `feat:` | New endpoint, feature, or capability |
| `fix:` | Bug correction |
| `chore:` | Dependency, config, or tooling change |
| `refactor:` | Code restructure without behavior change |
| `docs:` | Documentation only — no source code changes |

### Examples

```
feat: implement refresh token rotation on /auth/refresh
fix: exclude passwordHash from user response DTO
chore: upgrade Prisma to v7
refactor: extract token validation into shared guard
docs: update project docs for phase 3 completion
```

---

## Staging and Committing

- **Claude Code runs `git add` only** — never `git commit`
- The user executes all commits manually
- Every implementation prompt or command that stages files must provide the commit message explicitly so the user can copy it directly

---

## Branch Management

- Do not switch Git branches during a task — work on the currently checked-out branch
- Branch strategy is defined per project in `CLAUDE.md`

---

## What NOT to Do

- Never commit `.env` files or any file containing secrets
- Never commit files in `.claude/tmp/` — it is git-ignored by convention
- Never amend or rebase commits that have already been pushed
