# File Operations Skill

> **Note:** This is a translated version of the original Japanese skill. It has not been independently tested in English.

## Mono (Object)
- Operation type (operation): string · required
  One of: read / write / move / copy / delete / list
- Target path (path): string · required · cannot be empty
- Destination path (destination): string · required only for move and copy
- Content (content): string · required only for write
- Overwrite permission (overwrite): boolean · optional · default false

## Koto (Process)
Check ① → Check ② → Check ③ → Execute file operation

## Imi (Intent · Check Conditions)

### Check ① (On Start)
- Does the target path exist? (for read)
- Is the operation type one of the allowed list?
- Is execution permission available?

### Check ② (During Execution)
- Is path within an allowed folder?
  (System folders, .env files, and files containing API keys are prohibited)
- For delete: does the target exist?
- For write: if overwrite=false and the file already exists, abort
- For move: is destination within an allowed folder?
- Recursive deletion (equivalent to rm -rf) is unconditionally prohibited

### Check ③ (On Confirmation)
- Only transitions to executable state if checks ① and ② fully pass
- delete, move, and overwrite must always request confirmation before execution
- Irreversible operations (delete) require especially careful confirmation
- If any single item fails, do not execute · report the reason

## Prohibited Actions
- Execution without passing check ③
- Accessing system folders
- Accessing .env files, API keys, or credential files
- Recursive deletion
- delete, move, or overwrite without confirmation
