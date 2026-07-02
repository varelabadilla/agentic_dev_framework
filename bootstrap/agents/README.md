# agents/

Specialized Claude Code sub-agents for post-implementation review. Copy the entire `agents/` folder to `.claude/agents/` in any new project.

Agents are invoked from the main Claude Code session using natural language:

- "Run the architect agent to review my changes"
- "Run a security review on this branch"

---

## Available Agents

### `architect.md`

Reviews code changes for architecture quality.

**Checks:**
- Clean Architecture — business logic in services, controllers only route
- SOLID principles — single responsibility, dependency inversion, etc.
- NestJS patterns — module boundaries, DTOs, guards, dependency injection
- Code quality — error handling, no hardcoded values, focused methods
- Project-specific conventions from `CLAUDE.md` and `project-tech-standards-nodejs.md`

**Output:** Written to `.runbook/arch-status.md`. Findings categorized as BLOCKER / SUGGESTION / NIT.

**Allowed tools:** Read, Grep, Glob, Bash (read-only)
**Disallowed tools:** Write, Edit (never modifies files)

---

### `security-reviewer.md`

Reviews code changes for security vulnerabilities.

**Checks:**
- Secrets and credentials — hardcoded values, private key exposure, sensitive fields in responses
- Authentication and authorization — missing guards, weakened JWT validation, role bypass
- Cryptography — bcrypt usage, raw token storage, RSA key length, symmetric signing
- Input validation — missing DTO validation, unvalidated UUIDs, unsanitized user input
- Token and cookie security — httpOnly, token rotation, revocation on logout
- Multi-tenancy — missing tenant scope filters, cross-tenant data leaks (if applicable)
- Project-specific security rules from `project-security-standards.md` (if present)

**Output:** Written to `.runbook/sec-status.md`. Findings categorized as BLOCKER / SUGGESTION / NIT.

**Allowed tools:** Read, Grep, Glob, Bash (read-only)
**Disallowed tools:** Write, Edit (never modifies files)

---

## When to Run Agents

Run both agents after `/execute` completes and before running `/docs`:

1. `/execute phase-N` — implementation complete
2. "Run the architect agent" — review for architecture issues
3. "Run the security reviewer" — review for security issues
4. Resolve any BLOCKERs before committing
5. `/docs phase-N` — update documentation

---

## Extending Agents

To add project-specific review criteria, add them to:
- `.claude/rules/project-tech-standards-nodejs.md` — the architect agent reads this
- `.claude/rules/project-security-standards.md` — the security agent reads this

Do not modify the agent files directly for project-specific rules — keep the agents generic and put project context in the rule files.
