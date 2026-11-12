# Breach Responder — UGREEN AI NAS Hackathon 2026 Submission

**Skill name:** `breach-responder`
**Track:** 04 Open Innovation
**Package:** `breach-responder-1.0.0.zip` (contains `breach-responder/SKILL.md` and four linked files in `breach-responder/references/`)
**Static checker:** the official `uliya-skill-kit` checker reports `static-pass` with 0 errors and 0 warnings.

---

## 1. Project Proposal

### The problem

Data breaches are now routine. The Identity Theft Resource Center counted a record 3,332 US data compromises in 2025, and 70% of the breach notices sent to victims did not say what kind of attack happened ([Infosecurity Magazine, reporting ITRC's 2025 annual report](https://www.infosecurity-magazine.com/news/us-data-breaches-record-high/)). The notice tells you that something went wrong. It rarely tells you what *you* are exposed to.

The hard part for an ordinary person is the question that comes right after: *"Wait… what did I ever give that company?"* Over the years people sign up, upload ID scans, receive invoices, fill in claim forms and take screenshots, and all of it ends up scattered across folders on their NAS, often with names like `scan_004.jpg`. When a breach notice arrives, there is no fast way to reconstruct their own exposure, so most people either panic or do nothing.

At the same time, reviewers say AI on NAS devices still has to prove itself. AppleInsider's review of the iDX6011 Pro says local AI is worth having "even if the AI isn't great yet" ([AppleInsider](https://appleinsider.com/articles/26/03/24/ugreen-nasync-idx6011-pro-nas-review-the-right-kind-of-overkill)), and NASCompares notes the assistant "sometimes returning incomplete or incorrect results when it cannot find enough relevant material" ([NASCompares](https://nascompares.com/2026/01/21/ugreen-idx6011-pro-ai-nas-review/)). Breach Responder answers that with a concrete, checkable result: every finding points to a real file, and anything uncertain is labelled as uncertain.

### Who has this problem

Anyone with years of personal paperwork on a NAS: families, freelancers and small-business owners who keep ID scans, insurance forms, invoices and account screenshots at home. These are exactly the people who would **never** upload that pile to a cloud AI to answer a breach question, which is why this job belongs on the NAS.

### What the Skill does

The user types a company name (plus any other names they know, such as a brand or website domain) and chooses the folders the Skill may search. Breach Responder then:

1. **Finds** every file linked to that company in three ways: by file or folder name, by document content, and by the text visible in photos and screenshots. This catches the invoice called `INV-554.pdf` and the ID scan called `scan_004.jpg`, not just files with the company in their name.
2. **Lists which types of personal data** those files contain (full name, address, phone, email, date of birth, passport or national ID, payment-card details, bank account, login credentials, tax identifiers, signatures or photographed ID, health or insurance identifiers), each cited to its source file with a confidence level.
3. **Keeps a separate "needs a human look" list** for unreadable, encrypted or unclear files, instead of guessing.
4. **Builds an action checklist**, highest risk first (credentials, then payment data, then identity documents), with every item tied to the file behind it.
5. **Saves the report** to the NAS as a new file, only after the user approves.

### How it works

Breach Responder is a pure Uliya **orchestration** Skill: one `SKILL.md` plus four linked reference files. It never runs NAS operations itself. It hands each stage to the registered expert that owns that capability and passes only real results from one stage to the next.

| Stage | Delegated expert | Capability | Effect |
|---|---|---|---|
| 1a. Find files by name or folder | `file-agent` | `ug-filesystem` | read-only |
| 1b. Find files by document content | `knowledge-agent` | `ug-knowledge` | read-only |
| 1c. Find photos and screenshots by visible text | `photo-agent` | `ug-photo` | read-only |
| 2. Classify personal data in documents | `document-agent` | `ug-document` | read-only |
| 3. Classify personal data in images | `photo-agent` | `ug-photo` | read-only |
| 4. Build the report and checklist | (the Skill itself, in chat) | — | no change |
| 5. Save the report | `knowledge-agent` | `ug-knowledge` | new file, after approval |

The three searches in stage 1 are independent, so one failing does not stop the others. Results are merged and de-duplicated, and the report shows which search found each file. Documents and images are processed in batches of at most 20 files.

Capability evidence declared in the Skill, matching the official catalog: `ug-filesystem → file-agent`, `ug-knowledge → knowledge-agent`, `ug-photo → photo-agent`, `ug-document → document-agent`.

### What makes it different

- **It starts from a real-life trigger, not from a file type.** Most NAS AI features organise photos or summarise documents. Breach Responder starts from a breach notice and answers the user's exposure to one specific company.
- **Local processing is the product, not a footnote.** The whole value depends on the files never leaving the NAS: nobody would upload passport scans and card invoices to a cloud service just to answer this.
- **Checkable, not hand-wavy.** Every finding cites a source file and a confidence level, and the report says plainly what it could not check.

### Privacy and safety design

- **Authorized folders only.** All three searches stay inside the folders the user chooses for that run. The Skill never widens the scope or guesses a path.
- **Read-first.** Finding and classifying are read-only. The only change is saving the report as a new file, after explicit approval. Nothing is moved, renamed, tagged, overwritten or deleted.
- **Type and location, never the value.** The Skill reports "passport number found in `forms/acme-claim-id-scan.jpg`", never the number itself, not even the last digits. The saved report is therefore safe to keep on the NAS.
- **Honest results.** It handles `completed`, `failed`, `cancelled`, `waiting_user` and `waiting_approval` explicitly. An empty search is reported as a valid answer, unreadable files go to the needs-human-review list, and nothing is invented.
- **Honest about its limits.** Every report ends by saying it covers only files on the NAS and cannot confirm whether the user's data was part of the breach.
- **Local only.** All work runs through registered experts on the NAS: no external services, uploads, network calls or embedded credentials.

### Status and what is untested

The package passes the official static checker with 0 errors and 0 warnings. These can only be confirmed on a real UGREEN AI NAS and are therefore **untested**: installation and upload to Skills Hub; runtime execution of each delegated expert; search and OCR quality on real files; approval-flow behaviour at runtime; and performance on very large folders. We have prepared a test set of clearly fake documents and screenshots, with expected results, to validate all of this on a device during the finals. The demo video is a prototype walkthrough with sample data, not a recording of a device run.

---

## 2. Demo video script (as recorded, 2:00)

Final narration of `breach-responder-demo.mp4`. Use it as-is, or record your own voice over the same timings.

**[0:00–0:05] Title card**
- VO: Meet Breach Responder: an AI Skill for UGREEN AI NAS.

**[0:05–0:14] Ask screen**
- VO: So, you get an email. Acme Insurance has had a data breach. And your first thought is... wait, what did I actually give them?

**[0:14–0:23] Ask screen**
- VO: Years of forms, invoices and screenshots are sitting on your NAS. Digging through all of that by hand? That takes hours.

**[0:23–0:36] Ask screen**
- VO: Breach Responder answers that question using only the files on your NAS. Nothing leaves the device. Because honestly, you'd never upload your passport scans to a cloud AI, just to check.

**[0:36–0:44] Ask screen**
- VO: You just type the company name, any other names you know it by, and the folders it's allowed to search. That's it.

**[0:44–0:54] Find screen**
- VO: First, it searches three ways at once: by file name, by what's written inside your documents, and by the text in your photos and screenshots.

**[0:54–1:06] Find screen**
- VO: So it catches files like this invoice, I-N-V five fifty-four, and this I-D scan, scan zero zero four. Not just the ones with Acme in the name.

**[1:06–1:14] Classify screen**
- VO: Next, it reads each file, and lists which types of personal data show up, each with its source file and a confidence level.

**[1:14–1:23] Classify screen**
- VO: And it never shows the actual card or passport number. Not even the last four digits. Just the type of data, and where it lives.

**[1:23–1:34] Review screen**
- VO: Anything unclear or unreadable goes into a needs-a-human-look list, instead of being guessed. And if nothing matches, it simply says so. It never makes things up.

**[1:34–1:41] Checklist and approval screen**
- VO: Then you get a checklist, highest risk first, with every item tied to the file behind it.

**[1:41–1:49] Checklist and approval screen**
- VO: The report is only saved to your NAS after you approve it. Nothing gets moved, and nothing gets deleted.

**[1:49–1:55] Report saved screen**
- VO: Breach Responder. Your data, your answer, on your NAS.

**[1:55–2:00] End card**
- VO: (none) — closing card with the tagline and credits
