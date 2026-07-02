# Phase Workflow

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project using the agentic-dev-framework phase-based development cycle.

---

## The Cycle

```
/plan → /plan-adjust → /approve → /execute → /docs → /clean-up
```

Optional, before the cycle:
```
/proposal → /proposal-to-phase → /proposal-approve
```

---

## Phase Documents

- Every phase requires a **dedicated plan document** with unchecked task items before any execution begins
- Phase files live in `docs/phases/phase-NN-N-slug.md`
  - `NN` is zero-padded to two digits
  - Sub-phases use a hyphen separator: `02-5` for phase 2.5
  - The slug is derived from the phase name: lowercase, words separated by hyphens
- When a new phase is added, create its file in `docs/phases/` and add a row to `docs/development_plan.md`
- Never add task detail directly to `development_plan.md` — it is an index only

### Task item states

Every task item in a phase file uses one of three states:

| Syntax | Meaning |
|---|---|
| `- [ ]` | Pending — not yet started |
| `- [x]` | Complete — implemented and verified |
| `- [-]` | Ignored — skipped by explicit user decision |

A `- [-]` item must always have a brief note explaining why it was skipped.

---

## Planning Rules

- **Planning and execution are always separate** — `/plan` generates the plan, `/execute` implements it
- `/approve` confirms the plan — it never triggers execution
- `/execute` never runs without an approved `plan.md` in `.runbook/`
- Test coverage analysis is part of every plan — never deferred to a separate QA phase

---

## Definition of Done

Every phase is complete only when **all** of the following are true:

- [ ] All automated tests pass
- [ ] No `- [ ]` items remain in the phase file without justification (use `- [-]` for intentionally skipped items)
- [ ] `docs/dev_status.md` updated
- [ ] `docs/decision_log.md` updated with D-XXX entries for this phase
- [ ] `docs/technical_decisions.md` reviewed — pending decisions table updated if applicable
- [ ] `docs/development_plan.md` marks the phase as `✅ Complete`
- [ ] All changed files staged with `git add`, commit message provided

---

## Phase File Structure

Every phase file must follow this structure:

```markdown
# Phase N — Name

**Goal:** One sentence describing what this phase accomplishes.

### Section Name (group related tasks)

- [ ] N.1. Task description
- [ ] N.2. Task description

---

> **Phase N completed — {Month Year}**
> Deviations from original spec:
> - {description, or "None"}
```

---

## Completing a Phase

1. In the phase file: mark all completed tasks as `[x]`, ignored tasks as `[-]`, append the completion block
2. In `development_plan.md`: update the Status column to `✅ Complete`
3. Run `/docs phase-N` to update all project documentation
4. Run `/clean-up` to clear `.runbook/` before starting the next phase

**Never mark the next phase as "In Progress"** when completing the current one. The next phase status is updated only when that phase is explicitly started.

---

## Deferred QA

If QA cases require conditions unavailable during implementation (live credentials, external services):

- The phase status remains `✅ Complete`
- A new sub-phase file is created: `docs/phases/phase-NN-N-slug-qa.md`
- The sub-phase file cross-references the original phase and writes results back into it upon completion

---

## What NOT to Do

- Never execute implementation steps inside a `/plan` command
- Never start `/execute` without a `plan.md` approved via `/approve`
- Never defer test coverage to a phase after implementation
- Never mark a phase complete without updating all documentation files
