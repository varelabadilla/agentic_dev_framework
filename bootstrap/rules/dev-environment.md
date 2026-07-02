# Development Environment

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project running a local development server.
> Project-specific values (port number, start command) are defined in `CLAUDE.md`.

---

## Server Lifecycle

### Before starting the server

Check whether the project port is already in use. The port number is defined in `CLAUDE.md`.

**macOS / Linux:**
```bash
lsof -ti:<PORT>
```

**Windows (PowerShell):**
```powershell
Get-NetTCPConnection -LocalPort <PORT> -ErrorAction SilentlyContinue | Select-Object -ExpandProperty OwningProcess -Unique
```

If a process is found on the port, **inform the user and wait for confirmation** before stopping it — it may belong to work outside this task. Do not stop it silently.

### Starting the server

Use the start command defined in `CLAUDE.md` for this project.

### After the task is complete

Stop **only the server process started for this task** — identified by its PID or the terminal/job it was launched in.

If falling back to killing whatever holds the port, confirm with the user first.

**macOS / Linux:**
```bash
lsof -ti:<PORT> | xargs kill -9
```

**Windows (PowerShell):**
```powershell
Get-NetTCPConnection -LocalPort <PORT> -ErrorAction SilentlyContinue | ForEach-Object { Stop-Process -Id $_.OwningProcess -Confirm -ErrorAction SilentlyContinue }
```

---

## Temporary Files and Scratchpad

- Use **`.claude/tmp/`** as the scratchpad directory for all intermediate files, generated SQL, step files, and temporary output
- This directory is **git-ignored by convention** — never commit its contents
- **Never use system temp directories** (`AppData`, `/tmp`, `%TEMP%`, etc.) for task-related files
- Clean up `.claude/tmp/` contents after each task if they are no longer needed

---

## What NOT to Do

- Never stop a process that was not started by the current task without explicit user confirmation
- Never write intermediate or temporary files to system directories
- Never leave the development server running after the task is complete
