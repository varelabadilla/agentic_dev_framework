# /update-structure — Create or Update STRUCTURE.md

## Usage

`/update-structure`

Run this command:
- After initializing a new project (`/init-project`)
- After completing a phase that adds, moves, or removes files
- Any time the repository structure changes significantly

## Process

1. Run the following command to get the current file system state:

   ```bash
   find . \
     -not -path '*/node_modules/*' \
     -not -path '*/.git/*' \
     -not -path '*/dist/*' \
     -not -path '*/prisma/migrations/*' \
     -not -path '*/.DS_Store' \
     -not -path '*/__pycache__/*' \
     -not -path '*/.next/*' \
     -not -path '*/coverage/*' \
     -not -path '*/build/*' \
     | sort
   ```

   Do not generate any content until the command has completed and you have reviewed the full output.

2. Using the output from step 1, generate `STRUCTURE.md` at the repository root containing a single ASCII tree diagram.

   **Rules:**
   - Reflect the current state of the file system exactly — no additions, no omissions
   - Exclude: `node_modules/`, `dist/`, `.git/`, `prisma/migrations/`, `.DS_Store`, `coverage/`, `build/`, `.next/`, `.runbook/`, `.claude/tmp/`, and any other auto-generated or tooling-managed directories
   - Include: all source files, config files, `.md` files, and any files created as part of this project
   - Use indented ASCII tree format with `├──` and `└──` characters consistently
   - Do not add descriptions, comments, or explanations next to file names — just the tree
   - The root node of the tree is the repository folder name (derive from the current directory name)

   **Example format:**
   ```
   my-project/
   ├── .claude/
   │   ├── commands/
   │   │   └── plan.md
   │   └── rules/
   │       └── code-standards-nodejs-typescript.md
   ├── docs/
   │   ├── phases/
   │   │   └── phase-01-schema-database.md
   │   └── development_plan.md
   ├── src/
   │   ├── auth/
   │   │   └── auth.service.ts
   │   └── main.ts
   ├── .env.example
   ├── .gitignore
   ├── CLAUDE.md
   ├── package.json
   └── STRUCTURE.md
   ```

3. **Verify before writing:**
   - [ ] Every file in the tree was found in the `find` output — nothing invented
   - [ ] Excluded directories are not present in the tree
   - [ ] Tree uses consistent `├──` / `└──` / `│` characters throughout
   - [ ] No file descriptions or comments appear next to file names
   - [ ] Root node matches the actual repository folder name

4. Write the file to `STRUCTURE.md` at the repository root.
   If `STRUCTURE.md` already exists, overwrite it entirely.

5. Stage the file:
   ```bash
   git add STRUCTURE.md
   ```

6. Append to `activity.log`:
   `[{timestamp}] /update-structure: STRUCTURE.md updated`

7. Inform the user:
   "STRUCTURE.md updated.
   Commit message: `chore: update STRUCTURE.md`"

## Restrictions

- Do not add descriptions next to file names
- Do not invent files that do not exist
- Do not omit files that do exist (except excluded directories)
- All content in English
