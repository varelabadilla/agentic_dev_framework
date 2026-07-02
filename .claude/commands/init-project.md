# /init-project — Initialize a New Project from the Framework

## Usage

`/init-project`

Run this once after forking the `agentic-dev-framework` repository to set up a new project.

## Safety Check

1. Check if `bootstrap/` exists in the current directory.
   - If `bootstrap/` does **not** exist → inform the user:
     "This project has already been initialized. bootstrap/ no longer exists.
     If you need to re-initialize, restore bootstrap/ from the framework repository first."
     Stop.

## Gather Project Information

2. Ask the user the following questions one at a time. Wait for each answer before asking the next:

   - **Project name:** What is the name of this project? (e.g. "LearningTracker", "ERP Futuro")
   - **Project description:** One sentence describing what this project does.
   - **Tech stack:** What is the primary stack? (e.g. NestJS + TypeScript + Prisma + PostgreSQL)
   - **Development port:** What port will the development server run on? (e.g. 3000, 3001, 8080)
   - **Author / team:** Who is the developer or team name?

## Initialize Project Structure

3. Create the following directory structure:

   ```
   docs/
   ├── phases/
   ├── proposals/
   ├── diagrams/
   └── planner/
   .claude/
   ├── rules/
   ├── commands/
   ├── agents/
   └── tmp/
   .runbook/
   ```

4. Copy framework files to their project destinations:

   | Source | Destination |
   |---|---|
   | `bootstrap/rules/*.md` | `.claude/rules/` |
   | `bootstrap/commands/*.md` | `.claude/commands/` |
   | `bootstrap/agents/*.md` | `.claude/agents/` |
   | `bootstrap/templates/CLAUDE-template.md` | `CLAUDE.md` |
   | `bootstrap/templates/decision-ai-log-template.md` | `decision-ai-log.md` |
   | `bootstrap/templates/phase-template.md` | `docs/phases/phase-template.md` |
   | `bootstrap/templates/project-setup-template.md` | `docs/planner/project-setup.md` |
   | `bootstrap/templates/project-security-standards-template.md` | `.claude/rules/project-security-standards.md` |
   | `bootstrap/templates/project-tech-standards-nodejs-template.md` | `.claude/rules/project-tech-standards-nodejs.md` |

5. Personalize `CLAUDE.md` using the answers from step 2:
   - Replace `{PROJECT_NAME}` with the project name
   - Replace `{PROJECT_DESCRIPTION}` with the project description
   - Replace `{TECH_STACK}` with the tech stack
   - Replace `{PORT}` with the development port
   - Replace `{AUTHOR}` with the author/team name

6. Overwrite `README.md` completely. It currently describes the framework itself (forked as-is into this repository) — replace its entire content with the minimal project README below, discarding everything it had before:

   ```markdown
   # {PROJECT_NAME}

   {PROJECT_DESCRIPTION}

   ## Stack

   {TECH_STACK}

   ## Getting Started

   See CLAUDE.md for development conventions and workflow.
   ```

7. Create `.gitignore` entries if `.gitignore` exists — append if not already present:

   ```
   .env
   .runbook/
   !.runbook/activity.log
   .claude/tmp/
   ```

8. Confirm deletion before removing anything. Inform the user exactly which files/directories will be permanently deleted:

   - `bootstrap/` and all its contents (templates already copied to their destinations in steps 4-7)
   - `.claude/commands/init-project.md` (this command — one-time use, not needed after initialization)

   Ask the user to type `yes` to confirm. Do not proceed to step 9 unless the user's reply is exactly `yes`. If the user does not confirm, stop here and leave `bootstrap/` and `.claude/commands/init-project.md` untouched — the user can re-run `/init-project` later to retry.

9. Delete `bootstrap/` and its contents, and delete `.claude/commands/init-project.md` — neither is needed in the project repository after initialization.

10. Initialize `activity.log`:

   ```
   [{timestamp}] PROJECT INITIALIZED — {PROJECT_NAME}
   [{timestamp}] Framework: agentic-dev-framework
   ```

11. Stage all created and modified files:
    ```bash
    git add .
    ```

12. Inform the user:

    "Project initialized successfully.

    ## What was created

    - `.claude/rules/` — portable rule files
    - `.claude/commands/` — slash commands
    - `.claude/agents/` — specialized review agents
    - `docs/phases/`, `docs/proposals/`, `docs/diagrams/` — documentation structure
    - `CLAUDE.md` — personalized for {PROJECT_NAME}
    - `decision-ai-log.md` — ready to use
    - `activity.log` — time tracking initialized

    ## Next steps

    1. Complete `CLAUDE.md` — fill in Architecture Rules, Schema, Endpoints, and Environment Variables for this project
    2. Complete `.claude/rules/project-security-standards.md` — add project-specific security rules
    3. Complete `.claude/rules/project-tech-standards-nodejs.md` — add project-specific tech conventions
    4. Open `docs/planner/project-setup.md` — use it to configure your Claude.ai Project Setup
    5. Create `docs/development_plan.md` with your initial phase list
    6. Commit: `chore: initialize project from agentic-dev-framework`

    ## Files requiring manual completion

    - CLAUDE.md (Architecture Rules, Schema, Endpoints, Environment Variables)
    - .claude/rules/project-security-standards.md
    - .claude/rules/project-tech-standards-nodejs.md
    - docs/planner/project-setup.md → configure and upload to Claude.ai project"
