# /proposal-to-phase — Convert a Proposal into a Formal Phase

## Usage

`/proposal-to-phase {path-to-proposal}`

Example: `/proposal-to-phase docs/proposals/proposal-001.md`

## Process

1. Parse `$ARGUMENTS` to extract the proposal file path.
   If the file does not exist, inform the user and stop.

2. Read the proposal file in full.

3. **Validation — assess whether the proposal has sufficient detail:**

   Check for:
   - A clear, specific summary (not just a vague idea)
   - At least one concrete proposed approach
   - No blocking open questions that would prevent task definition

   If validation fails:
   - List exactly what is missing or unresolved
   - Inform the user: "This proposal needs more detail before it can become a phase. See above."
   - Stop. Do not generate a phase file.

4. Read the following files in full:
   - `docs/development_plan.md` — determine the next available phase number and understand existing phases
   - `docs/decision_log.md` — identify constraints
   - `docs/information_architecture.md` — understand affected entities
   - `CLAUDE.md` — apply project conventions

5. Search `src/` for files directly relevant to the proposal scope.
   Read any relevant service, module, controller, or schema files.

6. Determine the next available phase number from `docs/development_plan.md`.
   Use the same numbering and slug conventions defined in `.claude/rules/phase-workflow.md`.

7. Generate the phase file draft following the standard phase file structure:

   ```markdown
   # Phase N — {Name}

   **Goal:** {One sentence describing what this phase accomplishes.}

   ### {Section}

   - [ ] N.1. {Task}
   - [ ] N.2. {Task}

   ### QA

   - [ ] N.X. {Manual verification case}
   ```

   Tasks must be:
   - Derived from the proposal's proposed approach
   - Informed by existing code and architecture
   - Specific enough to be actionable by `/plan-execute`

8. Generate the `development_plan.md` row for this phase:

   ```
   | N | {Name} | {Goal — same as phase file} | ⏳ Pending |
   ```

9. Present the draft phase file and the `development_plan.md` row to the user.
   Inform them:
   "Phase N draft generated from proposal-{NNN}.md.

   Review the draft above and run **/proposal-approve** to write the files,
   or tell me what to adjust before approving."

10. **Do NOT write any files yet.** Wait for `/proposal-approve`.

11. Append to `activity.log`:
    `[{timestamp}] /proposal-to-phase: Phase N draft generated from proposal-{NNN}.md — awaiting approval`
