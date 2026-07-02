# Project Tech Standards — Node.js — {PROJECT_NAME}

> Project-specific technical conventions for Claude Code.
> These rules extend the portable rules in `.claude/rules/` — do not duplicate what is already there.
> Complete all {placeholders} and remove sections that do not apply.

---

## Module Structure

Each domain module follows this pattern:

```
src/{module-name}/
├── {module-name}.module.ts
├── {module-name}.controller.ts
├── {module-name}.service.ts
└── dto/
    ├── create-{name}.dto.ts
    └── update-{name}.dto.ts
```

Read the existing module structure before adding new files. Do not deviate from this pattern.

---

## Prisma Conventions

- Always read `prisma/schema.prisma` before writing any Prisma query
- Import PrismaClient from `{generated path}` — never from `@prisma/client`
- Generator provider is `"{generator}"` (not `"prisma-client-js"`)
- Never run `prisma migrate dev` unless explicitly instructed in the phase plan
- Only run `prisma generate` after schema changes

---

## Business Rules

{List the key business rules that Claude Code must enforce. Examples:}

- A {Entity} is unique per ({field1}, {field2}) — the same {key} can exist in multiple {context} combinations
- A {junction record} must exist before {dependent record} can be created
- {Role} users cannot {action} that are not {constraint}
- On {event}, all existing {related records} must be {action}

---

## Deletion Strategy

- Logical deletion via `isActive: false` — never physically delete {entity} records
- {Token/session records} use `{field}` for invalidation, not deletion
- Before implementing any deactivation logic, read the current service file to understand downstream dependencies

---

## API Design

- New endpoints must include Swagger decorators: `@ApiTags`, `@ApiOperation`, `@ApiResponse`
- Swagger is {enabled/disabled} in production — {how it is handled}
- When a feature requires a different query shape, create a new endpoint — do not modify existing ones

---

## Testing

- {Test type} tests live in `{path}` and use {framework}
- Run tests with: `{command}`
- Do not delete or skip existing tests — fix them alongside the implementation when a change breaks them

---

## How to Run Locally

```bash
{install command}
{db setup command}
{seed command}
{start command}
```

Server: `http://localhost:{PORT}`
{Swagger or docs URL if applicable}

---

## What NOT to Do

- Do not switch Git branches — work on the currently checked-out branch
- Do not hardcode UUIDs or credentials in service logic
- Do not run `{dangerous command}` unless explicitly instructed
- Do not couple this service to {other system}
- Do not expose {sensitive field} in any response
- Do not store {calculated value} as a database column
