# Privacy and safety rules for every step

These rules bind every delegation. They keep the workflow local, read-first and honest.

## Authorized scope only

- Work only inside the folders the user authorized for this run, in every search (file names, document content, photos and screenshots).
- If no folder was authorized, ask and wait. Never guess a path.
- Never widen the scope or switch to another folder or data source to force a result.

## Read-first, one approved change

- Steps 1 to 4 are read-only. They never move, rename, copy, tag or delete anything.
- The only change is saving the report as a new file in step 5. Show the user what will be written and where, and save only after explicit approval.
- Never overwrite an existing file. If the name exists, report it and wait for a new name.
- If the user declines, keep the report in chat and save nothing.

## Never the value

- Report personal data by category and source file only, as defined in [pii-taxonomy.md](pii-taxonomy.md).
- Never quote, return, display or save a card number, security code, bank account number, passport or national ID number, password or security answer, in full or in part. This applies to chat messages and to the saved report.

## Honest results

- Use a delegation's result only after it returns `completed`. Report `failed`, `cancelled`, `waiting_user` and `waiting_approval` as states, never as success.
- An empty result is a valid answer. Say so plainly and stop the steps that depend on it.
- Pass only real paths and findings from earlier steps into later steps. Never invent files, matches or exposure findings.
- Cite the source file for every fact in the report.
- The report covers only files on the NAS. It cannot confirm whether the user's data was part of the breach, and it says so.

## Local only

- All work runs on the NAS through registered experts. No external services, uploads, network calls or embedded credentials.

## Large folders and edge cases

- Send documents and images in batches of at most 20 files per delegation, and tell the user how far the run has got.
- Empty folders, unsupported file types, and encrypted, damaged or unreadable files go to the needs-human-review list with the reason, never guessed.
