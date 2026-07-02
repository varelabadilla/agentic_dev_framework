# /plan-docs — Update Project Documentation After Phase Completion

## Usage

`/plan-docs phase-N`

## Process

1. Parse `$ARGUMENTS` to extract the phase number.

2. Read the following files in full before modifying anything:
   - `.runbook/implementation-results.md` — what was built and any deviations
   - `.runbook/plan.md` — the approved plan (commit messages, decisions made)
   - `docs/dev_status.md`
   - `docs/development_plan.md`
   - `docs/decision_log.md`
   - `docs/technical_decisions.md`

3. Update `docs/dev_status.md`:
   - Update "Current Phase" to the next phase (N+1)
   - Write a new narrative paragraph summarizing what Phase N completed, including deviations
   - Update the Phase Completion Status table: mark Phase N as `✅ Complete`
   - Clear resolved Known Issues; add any new ones from `implementation-results.md`

4. Update `docs/development_plan.md`:
   - Mark all Phase N checkboxes as `[x]`
   - If bugfixes were applied, add a BF-XXX block after the Phase N section:
     ```
     ## Bugfix — {description} (applied after Phase N QA)
     - [x] BF-00X. {what was fixed}
     - [x] BF-00X. Commit Message: `fix: {message}`
     ```

5. Update `docs/decision_log.md`:
   - Append a new D-XXX entry for every significant decision made during Phase N
   - Derive from `.runbook/plan.md` and `implementation-results.md`
   - D-XXX entries are always appended — never inserted between existing entries
   - If no new decisions were made, do not add an entry

6. Update `docs/technical_decisions.md`:
   - Update the Pending Decisions table if any item was resolved or newly added
   - If no changes, leave the table as-is

7. Verify all edited files:
   - No broken markdown tables
   - No unclosed code blocks
   - No duplicate section headings
   - Checkbox syntax is exactly `- [x]` or `- [ ]`

8. **Do NOT modify any source code files.**
   **Do NOT run `git add` or `git commit`.**

9. Write `.runbook/pm-status.md`:
   ```
   # PM Status — Phase N Docs Update

   ## Files Updated
   - docs/dev_status.md — {summary}
   - docs/development_plan.md — {summary}
   - docs/decision_log.md — {new entries added or "none"}
   - docs/technical_decisions.md — {summary or "no changes"}

   ## Status: COMPLETE
   ```

10. Append to `activity.log`:
    `[{timestamp}] /plan-docs phase-N: Documentation updated. Ready for commit.`

11. Present the docs commit message to the user:
    "Documentation updated. Commit message:
    `docs: update project docs for phase N completion`
    Run /plan-clean-up when ready to start Phase N+1."
