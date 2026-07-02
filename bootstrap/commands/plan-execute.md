# /plan-execute — Execute the Approved Implementation Plan

## Usage

`/plan-execute phase-N`

Where N matches the phase number planned with `/plan phase-N`.

## Process

1. Parse `$ARGUMENTS` to extract the phase number.

2. Check that `.runbook/plan.md` exists. If it does not, inform the user:
   "No approved plan found. Run /plan phase-N first, then /plan-approve."
   Stop.

3. Read `.runbook/plan.md` in full.

4. Read `CLAUDE.md` in full before making any changes.

5. Check the project port before starting the server (if needed for QA).
   The port is defined in `CLAUDE.md`. See `.claude/rules/dev-environment.md` for the check commands.
   If a process is found on the port, inform the user and wait for confirmation before stopping it.

6. Execute each implementation step from `.runbook/plan.md` in order:
   - Read every file before modifying it
   - Apply all conventions defined in `CLAUDE.md` and `.claude/rules/`
   - Never return sensitive fields (password hashes, raw tokens, private keys) in any response
   - Follow all security rules in `.claude/rules/dev-security-standards.md`

7. After all implementation steps, run:
   ```bash
   npm run build
   ```
   Fix any TypeScript errors before proceeding to QA.

8. Start the development server and run through the QA checklist in `.runbook/plan.md`.
   Document each result (PASS / FAIL + notes).

9. Stop the development server after QA. See `.claude/rules/dev-environment.md` for stop commands.

10. Write `.runbook/implementation-results.md`:

    ```
    # Implementation Results — Phase N

    ## Files Created
    - {path}

    ## Files Modified
    - {path} — {summary of changes}

    ## Schema Changes Applied
    {migration name, or "None"}

    ## Build
    PASS / FAIL

    ## QA Results
    {numbered list matching the QA checklist, each with PASS or FAIL + notes}

    ## Issues Found
    {bugs found and fixed during execution, or deviations from the plan, or "None"}
    ```

11. Append to `activity.log`:
    `[{timestamp}] /plan-execute phase-N: Implementation complete. Build: PASS/FAIL. QA: X/Y passed.`

12. Stage all changed files:
    ```bash
    git add .
    ```

13. Present the implementation results to the user and provide the commit message from `.runbook/plan.md`:
    "Implementation complete. Commit message:
    `{commit message from plan}`
    Run /docs phase-N when ready to update project documentation."
