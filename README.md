# Breach Responder

An AI Agent Skill for the **UGREEN AI NAS** that answers one question after a data breach:
*"What personal data did I ever give this company, and what should I do now?"*

Built for the **UGREEN AI NAS Hackathon 2026** (Track 04 — Open Innovation).

---

## What it does

When a company announces a data breach, Breach Responder scans the folders you authorize
on your NAS and produces a breach-exposure report that is cited to real source files:

1. **Finds** every file linked to the company in three independent ways — by file/folder name,
   by document content, and by the text visible in photos and screenshots. This catches files
   with unhelpful names like `INV-554.pdf` or `scan_004.jpg`, not just ones with the company
   name in them.
2. **Lists which types of personal data** appear in those files (payment-card details, passport
   or national ID, login credentials, home address, and more), each with its source file and a
   confidence level.
3. **Keeps a "needs human review" list** for unreadable, encrypted or low-confidence files,
   instead of guessing.
4. **Builds an action checklist**, highest risk first, each item tied to the file behind it.
5. **Saves the report** to the NAS as a new file — only after you approve.

## Privacy and safety by design

- **Authorized folders only.** Every search stays inside the folders you choose for that run.
  The Skill never widens the scope or guesses a path.
- **Read-first.** Finding and classifying are read-only. The only change is saving the report as
  a new file, after explicit approval. Nothing is moved, renamed, tagged, overwritten or deleted.
- **Type and location, never the value.** It reports *"passport number found in `claim-form-signed.pdf`"*,
  never the number itself — not even the last digits. The saved report is safe to keep on the NAS.
- **Honest results.** Empty searches are reported as valid answers, unreadable files go to the
  needs-human-review list, and nothing is invented.
- **Local only.** All work runs through registered experts on the NAS — no external services,
  uploads, network calls or embedded credentials.

## How it works

Breach Responder is a pure Uliya **orchestration** Skill: one `SKILL.md` plus four linked
reference files. It never runs NAS operations itself — it delegates each stage to the registered
expert that owns that capability and passes only real results from one stage to the next.

| Stage | Delegated expert | Capability | Effect |
|---|---|---|---|
| 1a. Find files by name or folder | `file-agent` | `ug-filesystem` | read-only |
| 1b. Find files by document content | `knowledge-agent` | `ug-knowledge` | read-only |
| 1c. Find photos/screenshots by visible text | `photo-agent` | `ug-photo` | read-only |
| 2. Classify personal data in documents | `document-agent` | `ug-document` | read-only |
| 3. Classify personal data in images | `photo-agent` | `ug-photo` | read-only |
| 4. Build the report and checklist | the Skill itself (in chat) | — | no change |
| 5. Save the report | `knowledge-agent` | `ug-knowledge` | new file, after approval |

## Repository layout

```
breach-responder-skill/
  breach-responder/
    SKILL.md                  # Core skill: the 5-step workflow
    references/
      action-checklist.md     # Actions per PII category, risk-ordered
      pii-taxonomy.md         # PII categories, confidence levels, never-the-value rule
      privacy-and-safety.md   # Scope, read-only, approval, local-only rules
      report-layout.md        # Report structure shown in chat and saved
test-pack/
  TEST-PLAN.md                # Finals test plan (T1–T8) with pass criteria
  Personal/                   # Fictional sample data (Documents + Screenshots)
visuals/
  breach-responder-diagram.html
  breach-responder-walkthrough.html
breach-responder-submission.md # Full hackathon writeup + demo script
breach-responder-system.drawio / .png
breach-responder-demo.mp4 / .en.srt
```

## Test data

All files under `test-pack/Personal/` are **fictional sample data**. The person (Jordan Sample),
address, `555-01xx` phone, `example.com` email, passport `SAMPLE0000` and the public test card
number `4111 1111 1111 1111` are all made up. See `test-pack/TEST-PLAN.md` for the full plan.

## Status

The package passes the official `uliya-skill-kit` static checker with 0 errors and 0 warnings.
Runtime behavior (installation, expert execution, OCR/search quality, approval flow) can only be
confirmed on a real UGREEN AI NAS and is validated during the finals using the included test pack.
