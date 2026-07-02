# /plan-clean-up — Clean Agent State and Close Session

## Usage

`/plan-clean-up`

Use this command:
- After completing a phase and committing both implementation and docs
- Before starting a new phase to ensure clean state
- When something unexpected happened and you want to start fresh

## Process

1. Check if `.runbook/` exists. If not, inform the user and stop.

2. List all files currently in `.runbook/` and show them to the user.

3. Ask for confirmation:
   "The following files will be DELETED from .runbook/:
   {list of files excluding activity.log}

   The following file will be PRESERVED:
   - activity.log (permanent project history — never deleted)

   Confirm? Reply **yes** to proceed or **no** to cancel."

4. If the user confirms with "yes":

   a. Read `activity.log` to find the most recent `SESSION START` entry without a matching `SESSION END`.
      Calculate session duration: endedAt = current timestamp, duration = endedAt - startedAt.
      Format as: `Xh Ym` or `Xm` if under one hour.

   b. Append to `activity.log`:
      `[{timestamp}] SESSION END — Duration: {duration}`

   c. Append to `activity.log`:
      `[{timestamp}] /plan-clean-up: Agent state cleaned. activity.log preserved.`

   d. Delete all files inside `.runbook/` **except** `activity.log`.
      Keep the `.runbook/` directory itself (it is gitignored).

   e. Inform the user:
      "Session closed. Duration: {duration}
      .runbook/ cleaned. activity.log preserved.
      Ready to start a new phase with /plan phase-N."

5. If the user replies "no":
   - Inform the user: "Cleanup cancelled. No files were deleted."
   - Stop.

## Note

`activity.log` is the permanent history of all work done on this project.
It is never deleted by any command. To review the full project history, read `activity.log` directly.
To see time totals across all sessions, run `/time-report`.
