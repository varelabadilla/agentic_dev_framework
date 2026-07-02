# /generate-postman — Generate or Update the Postman Collection

## Usage

`/generate-postman`

Run this command:
- After `/define-generate` when starting a new API project
- After `/plan-docs phase-N` when a phase added, modified, or removed endpoints

## Process

1. Check that `CLAUDE.md` exists and read it in full.
   Determine the project name, base URL, and port from `CLAUDE.md`.

2. Scan the source code to discover all current endpoints:

   ```bash
   grep -rn "@Get\|@Post\|@Put\|@Patch\|@Delete" src/ --include="*.ts"
   ```

   Also read all controller files found to extract:
   - HTTP method and path
   - Whether the endpoint is public or protected (look for `@Public()` decorator or guard usage)
   - Request body shape (from DTO imports and `@Body()` parameters)
   - Path parameters (from `@Param()` decorators)
   - Query parameters (from `@Query()` decorators)
   - Required headers (e.g. `Authorization`, `X-App-Secret`)

3. Check if `postman/` directory exists. Create it if it does not.

4. Check if `postman/{PROJECT_NAME}.postman_collection.json` already exists.
   - If it exists → read it in full, then update it with new/changed endpoints
   - If it does not exist → create it from scratch

5. Generate or update the Postman collection following this structure:

   ```json
   {
     "info": {
       "name": "{PROJECT_NAME}",
       "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
     },
     "variable": [
       { "key": "baseUrl", "value": "http://localhost:{PORT}" },
       { "key": "accessToken", "value": "" }
     ],
     "item": [
       {
         "name": "{Group name — e.g. Auth, Admin, SuperAdmin}",
         "item": [
           {
             "name": "{Endpoint name}",
             "request": {
               "method": "{HTTP METHOD}",
               "url": "{{baseUrl}}/{path}",
               "header": [...],
               "body": {...}
             }
           }
         ]
       }
     ]
   }
   ```

   **Collection rules:**
   - Group endpoints by controller or logical area (Auth, SuperAdmin, Admin, Public)
   - Public endpoints have no Authorization header
   - Protected endpoints include: `Authorization: Bearer {{accessToken}}`
   - Endpoints requiring a custom header (e.g. `X-App-Secret`) include it with a placeholder value
   - Request bodies use realistic placeholder values — not empty objects
   - Path parameters use Postman variable syntax: `{{userId}}`, `{{appId}}`
   - Collection-level variables include at minimum: `baseUrl`, `accessToken`
   - If the project uses refresh token cookies, add a note in the collection description

6. Write the collection to `postman/{PROJECT_NAME}.postman_collection.json`.

7. If a `postman/README.md` does not exist, create it:

   ```markdown
   # Postman Collection — {PROJECT_NAME}

   ## Setup

   1. Import `{PROJECT_NAME}.postman_collection.json` into Postman
   2. Set the `baseUrl` variable to your local server (default: `http://localhost:{PORT}`)
   3. After login, copy the `accessToken` from the response and set it as the
      `accessToken` collection variable

   ## Variables

   | Variable | Default | Description |
   |---|---|---|
   | `baseUrl` | `http://localhost:{PORT}` | Base URL of the running server |
   | `accessToken` | _(empty)_ | JWT access token — set after login |

   ## Endpoint Groups

   {List of groups in the collection with one-line description each}
   ```

8. Stage the files:
   ```bash
   git add postman/
   ```

9. Append to `activity.log`:
   `[{timestamp}] /generate-postman: Postman collection updated — {N} endpoints`

10. Inform the user:
    "Postman collection updated.
    File: postman/{PROJECT_NAME}.postman_collection.json
    Endpoints documented: {N}

    Commit message:
    `chore: update Postman collection`

    Import the file into Postman and set the collection variables before testing."

## Restrictions

- Do not modify any source code files
- Do not run `git commit` — stage only
- Do not invent endpoints that are not found in the source code scan
- All content in English
