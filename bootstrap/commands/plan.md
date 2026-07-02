# /plan — Generate Implementation Plan for a Phase

## Usage

`/plan phase-N`

Where N is the phase number (e.g. `/plan phase-5`).

## Process

1. Parse `$ARGUMENTS` to extract the phase number (e.g. "phase-5" → 5).

2. Create `.runbook/` if it does not exist.

3. Append to `activity.log` (session start record):
   `[{timestamp}] SESSION START — /plan phase-N`

4. Read the following files in full before generating anything:
   - `docs/development_plan.md` — extract all tasks for the requested phase
   - `docs/decision_log.md` — understand existing decisions that constrain implementation
   - `docs/information_architecture.md` — understand entity relationships
   - `CLAUDE.md` — apply project conventions and rules
   - `docs/dev_status.md` — confirm the phase is not already complete

5. If the phase is already marked complete in `dev_status.md`, inform the user and stop.
   Do not generate a plan for a completed phase.

6. Extract all tasks for the requested phase from `development_plan.md`.
   Include every sub-task (e.g. 5.1, 5.2, 5.1.1, 5.1.2).

7. **Test Coverage Analysis**

   For each method, class, or endpoint the plan will create, modify, or rename:

   1. Search for existing test files under `test/` and `src/` that reference that method, class, or endpoint.
   2. Identify which existing tests would break due to the planned changes.
   3. Include explicit plan tasks for:
      - Updating every affected test as part of the implementation (not as QA)
      - Creating new tests for every new method, class, or endpoint introduced
   4. Add these test tasks to the "Implementation Steps" section, adjacent to the implementation step they correspond to — not grouped at the end.

   **Note:** QA checklist items (manual verification) are separate from automated test tasks. Both must appear in the plan.

8. Generate the implementation plan using this structure:

   ```
   # Implementation Plan — Phase N: {Phase Name}

   ## Overview
   {1-2 sentences describing what this phase accomplishes}

   ## Files to Create
   - {path} — {why it is needed}

   ## Files to Modify
   - {path} — {what changes and why}

   ## Implementation Steps
   {Ordered list of concrete steps. Each step must reference the task number
   and include: what to do, which files are affected, any constraint from
   decision_log.md or CLAUDE.md, and any test tasks for that step.}

   ## Schema Changes
   {List Prisma model changes, new enums, migration name — or "No schema changes"}

   ## QA Checklist
   {Numbered list of cases to verify manually.}

   ## Commit Message
   {Conventional commit: feat:, fix:, chore:, or refactor:}

   ## Docs Commit Message
   docs: update project docs for phase N completion
   ```

9. Write the plan to `.runbook/plan.md`.

10. Append to `activity.log`:
    `[{timestamp}] /plan phase-N: Plan generated and presented for review`

11. Present the plan to the user and ask:
    "Does this plan look correct?
    - Run **/plan-approve** to confirm and then run /plan-execute phase-N
    - Run **/plan-adjust** to modify before approving
    - Run **/plan-discard** to discard and start over"

12. Wait for the user to run `/plan-approve`, `/plan-adjust`, or `/plan-discard`.
    **Do NOT execute any implementation steps.**
