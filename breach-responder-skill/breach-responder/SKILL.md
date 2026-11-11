---
name: breach-responder
description: Breach-exposure check for one named company. Finds the files, documents and screenshots in the folders you authorize that are linked to that company, lists which types of your personal data they contain with the source file for each, and prepares an action checklist and a report you can save. Use when a company you used announces a data breach, or when you ask what you ever shared with a company.
---

# Goal

When a company announces a data breach, help the user answer one question from their own NAS files: "What personal data did I ever give this company, and what should I do now?"

The result is a breach-exposure report, cited to real source files:

1. **Evidence list**: every file in the authorized folders linked to the company, found by file name, by document content, or by the visible text in a screenshot or photo.
2. **Exposure list**: which **types** of personal data appear in those files (for example payment-card details, passport or national ID, login credentials, home address), each with its source file and a confidence level.
3. **Needs human review**: unreadable, encrypted or low-confidence files, kept apart and never guessed.
4. **Action checklist**: what to change, revoke or monitor, highest risk first, each item tied to the file behind it.

Scope and limits:
- Works only inside the folders the user authorizes for this run.
- Reads and reports. The only change it can make is saving the report as a new file, and only after the user approves. It never moves, renames, tags, overwrites or deletes files.
- Reports the **type and location** of sensitive data, never the value. See [references/pii-taxonomy.md](references/pii-taxonomy.md).
- Only knows what is stored on this NAS. It cannot confirm whether the user's data was actually part of the breach, and the report says so.

# Required input

Ask only for what is missing:

- **Company name** (required). Also ask for other names the user knows: brand or app names, the legal entity name, the website or email domain (for example `Acme Insurance`, `AcmeCare`, `acme.com`). If none are given, use the company name alone and note in the report that other names were not searched.
- **Authorized folders** (required). One or more folders to search. If none are given, ask and wait. Never guess a folder path and never widen the scope.
- **Time range** (optional), for example "since 2019".
- **Where to save the report** (optional). If none is given, propose a folder and file name in step 5 and wait for approval.

Do not ask for card numbers, ID numbers, passwords or other secret values. Do not ask for file IDs, tag IDs or other expert parameters; the experts resolve them.

# Filling the instructions

Before each delegation, replace every `{…}` field with the real value:

- `{company}`, `{aliases}`, `{folders}`, `{time range}`: from the confirmed user input.
- `{document list}`, `{image list}`: only real file paths returned by step 1 in this run.
- `{save folder}`, `{file name}`, `{report text}`: from step 5, after the user approves them.

If an optional value is missing, remove the sentence that uses it. Never send a `{…}` field, an invented path, or a value an earlier step did not return.

# Generation-time capability evidence

- `ug-filesystem` → `file-agent`
- `ug-knowledge` → `knowledge-agent`
- `ug-photo` → `photo-agent`
- `ug-document` → `document-agent`

# Workflow

Read [references/privacy-and-safety.md](references/privacy-and-safety.md) before every run. Steps 1 to 4 are read-only. Step 5 is the only change and needs approval.

## Step 1: Find linked files (read-only, three independent searches)

Many relevant files have unhelpful names such as `scan_001.jpg` or `INV-554.pdf`, so search three ways. The three searches do not depend on each other.

**1a. By file or folder name**

```text
delegate --target file-agent --instruction 'Inside only these folders: {folders}, find files whose file name or folder path contains any of these terms, ignoring upper and lower case: {company}, {aliases}. Only include files modified within {time range}. Include documents, PDFs, spreadsheets, text files, images and screenshots. Read only: do not open, move, rename, tag or delete anything, and do not search outside these folders. Return the full path, file type, size and modified date of each match. If nothing matches, return an empty list and say so.'
```

**1b. By document content**

```text
delegate --target knowledge-agent --instruction 'Inside only these folders: {folders}, collect evidence of files whose content mentions the company {company}, also known as {aliases}. Only include files from {time range}. Look for sign-up or welcome emails, order confirmations, invoices, receipts, policies, contracts, claim or application forms, membership cards and support messages. Read only: do not change any file and do not search outside these folders. Return each source with its full path, file type, date if known, and one short sentence on why it is linked to {company}. Do not quote card numbers, ID numbers, account numbers or passwords. Also return coverage, failures and warnings. If nothing matches, return an empty result and say so.'
```

**1c. By text visible in photos and screenshots**

```text
delegate --target photo-agent --instruction 'Inside only these folders: {folders}, search photos and screenshots whose visible text contains any of these terms: {company}, {aliases}. Only include items taken or saved within {time range}. Typical matches are app or account screens, order or payment confirmations, photographed letters, cards and receipts. Read only: do not create albums, tag or change any photo, and do not search outside these folders. Return the full path and capture date of each match and the company name or term that was found. Do not return card numbers, ID numbers, account numbers or passwords. If nothing matches, return an empty result and say so.'
```

**Merge.** Combine the real results of 1a, 1b and 1c into one evidence list. Remove duplicate paths and record which search found each file (name, content, screenshot text). Split the list into documents (PDF, office, text) and images (photos, screenshots, scans).

If all three searches return empty results, tell the user: "No files linked to {company} were found in {folders}." Suggest other names or other folders the user may want to authorize, and stop. An empty result is a valid answer: do not widen the scope or continue with invented data.

## Step 2: Classify personal data in documents (read-only)

Run only if the evidence list has documents. Read [references/pii-taxonomy.md](references/pii-taxonomy.md) first. Send at most 20 files per delegation; for a longer list, send several batches.

```text
delegate --target document-agent --instruction 'Read each of these files: {document list}. For each file, identify which categories of personal data about the user appear in it, using only these categories: full name; home or mailing address; phone number; email address; date of birth; passport or national ID number; payment-card details; bank account; login credentials; government or tax identifier; signature or photographed identity document; health or insurance identifier. For each category found, return the file path, the page or section, and a confidence level of high, medium or low. Return the category and location only: never quote, return or partly reveal the actual value, such as a card, ID, account number or password. Read only. List every file you could not read, such as encrypted or damaged files, with the reason, and continue with the others.'
```

## Step 3: Classify personal data in photos and screenshots (read-only)

Run only if the evidence list has images. Send at most 20 images per delegation.

```text
delegate --target photo-agent --instruction 'Read the visible text in each of these images: {image list}. For each image, say whether it relates to the company {company}, and identify which categories of personal data about the user are visible, using only these categories: full name; home or mailing address; phone number; email address; date of birth; passport or national ID number; payment-card details; bank account; login credentials; government or tax identifier; signature or photographed identity document; health or insurance identifier. For each category found, return the image path and a confidence level of high, medium or low. Return the category and location only: never quote, return or partly reveal the actual value. Read only. If an image has no readable text, or the text is too unclear to judge, say so for that image instead of guessing.'
```

## Step 4: Build the report (in chat, read-only)

Using only the real results from steps 1 to 3:

- Put high- and medium-confidence findings in the exposure list, grouped by category, each with its source files.
- Put low-confidence findings and every unreadable or unclear file in the **Needs human review** list. They are not counted as confirmed exposure.
- Build the action checklist from [references/action-checklist.md](references/action-checklist.md), highest risk first, each item tied to its source file.
- Lay the report out as in [references/report-layout.md](references/report-layout.md).

Show the full report to the user in chat.

## Step 5: Save the report (changes data, needs approval)

Propose a folder and a new file name, for example `{company} breach report.md` in a folder the user authorized. Ask: "Save this report as a new file at this location?" Run the delegation only after the user says yes. If the user declines, keep the report in chat and save nothing.

```text
delegate --target knowledge-agent --instruction 'Save the Markdown text below as a new file named {file name} in the folder {save folder}. Create a new file only: if a file with that name already exists, do not overwrite it; report the conflict and wait for a new name. This writes data, so go through the normal approval flow before writing. Return the saved file path and whether the save was verified. Content: {report text}'
```

# Completion and failure

Use a result only after its delegation returns `completed`. Never report a step as done unless it returned `completed`.

- `completed`: use only the data returned. An empty result is a valid answer: say so plainly for that step, for example "No screenshots of {company} were found."
- `completed` with partial results: keep what was returned and list every failed or unreadable file under **Needs human review** with its reason. Do not claim the search was complete.
- `failed`: say which step failed and the reason given. Steps 1a, 1b and 1c are independent, so continue with the others and note the gap. If step 2 or 3 fails, list those files under **Needs human review**. If step 5 fails, the report in chat is still valid; say clearly that it was not saved. Do not retry silently or fill the gap with guesses.
- `cancelled`: stop that step, do not retry on your own, keep earlier verified results, and tell the user what was and was not done.
- `waiting_user`: pass the expert's question to the user word for word, and resume the same step only with the user's real answer.
- `waiting_approval`: expected before the step 5 save. Tell the user what will be written and where, and that nothing is saved until they approve. Do not report it as saved until the step returns `completed`.

The final message states:
- how many linked files were found, and by which search (name, content, screenshot text)
- the personal data categories confirmed, and how many files need human review
- whether the report was saved, with its path if saved
- this note: "This report is based only on files stored on your NAS. It cannot confirm whether your data was part of the breach. Check the company's official breach notice for that."

# References

- [references/privacy-and-safety.md](references/privacy-and-safety.md): scope, read-only default, approval, and local-only rules for every step. Read before every run.
- [references/pii-taxonomy.md](references/pii-taxonomy.md): the personal-data categories, confidence levels, and the never-the-value rule. Read before steps 2 and 3.
- [references/action-checklist.md](references/action-checklist.md): actions for each category and their risk order. Read before step 4.
- [references/report-layout.md](references/report-layout.md): the report structure shown in chat and saved in step 5.
