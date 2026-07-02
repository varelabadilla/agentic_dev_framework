# /time-report — Show Full Time Tracking Report

## Usage

`/time-report`

Generates a summary of all work sessions logged in `activity.log`.

## Process

1. Check that `activity.log` exists.
   If it does not, inform the user:
   "No activity log found. Start a phase with /plan phase-N to begin tracking time."
   Stop.

2. Read `activity.log` in full.

3. Extract all `SESSION START` and `SESSION END` pairs.
   For each completed session (has both START and END), capture:
   - Date
   - Phase or description (from SESSION START line)
   - Duration (from SESSION END line)

4. Calculate:
   - Total sessions completed
   - Total time across all sessions
   - Average session duration

5. Present the report in this format:

   ```
   ## Time Report

   ### Sessions

   | Date | Description | Duration |
   |------|-------------|----------|
   | {date} | {description} | {duration} |

   ### Summary
   - Total sessions: {N}
   - Total time logged: {Xh Ym}
   - Average session: {Xh Ym}
   ```

6. Note at the bottom:
   "This report reflects time logged via /plan (session start) and /plan-clean-up (session end) only.
   Time spent outside of logged sessions is not included."
