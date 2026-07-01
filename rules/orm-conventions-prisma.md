# ORM Conventions — Prisma

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project using Prisma as the ORM, regardless of database engine.

---

## Schema Conventions

- **Prisma does NOT apply snake_case automatically** — every model and field requires explicit mapping
- Every model must include `@@map("snake_case_table_name")` — no exceptions
- Every camelCase field must include `@map("snake_case_column_name")` — no exceptions
- Forgetting `@@map` or `@map` is a migration error that may require a reset — treat it as a blocker

---

## Client Import

- Always import PrismaClient from the **generated output path** defined in `schema.prisma`
- Never import from `@prisma/client` directly — the generated path is project-specific
- After any schema change, run `prisma generate` before running the application

---

## Migrations

- Never run `prisma migrate dev` unless explicitly instructed in the current phase plan
- Only run `prisma generate` after schema changes during normal development
- If a schema change is required, also update `prisma/seed.ts` and all affected service files

---

## Transactions

- Use `prisma.$transaction()` for **any operation that writes to more than one table**
- Multi-step mutations that must be atomic must always be wrapped in a transaction
- Never assume that sequential Prisma calls without a transaction are safe under concurrent load

---

## Query Patterns

- Always read `prisma/schema.prisma` before writing any Prisma query — field names and relations must match the schema exactly
- Use camelCase field names in Prisma queries — never the mapped snake_case name
- Use Prisma `select` or response DTOs to exclude sensitive fields from query results — never return the full model object from a controller

---

## What NOT to Do

- Never run `prisma migrate dev` without explicit instruction
- Never import from `@prisma/client` — use the generated path
- Never omit `@@map` or `@map` on any model or field
- Never perform multi-table writes outside of a `$transaction`
