# Personal-data categories and the never-the-value rule

Use these fixed categories when classifying what a company-linked file contains. Report the **category**, the **source file**, and a **confidence level**. Anything uncertain goes to the needs-human-review list instead of being guessed.

## Categories to detect

1. Full name
2. Home or mailing address
3. Phone number
4. Email address
5. Date of birth
6. Passport or national ID number
7. Payment-card details (card number, expiry date, security code)
8. Bank account (account number, IBAN)
9. Login credentials (username, password, security questions)
10. Government or tax identifier (for example social-security or tax number)
11. Signature or photographed identity document
12. Health or insurance identifier

## The never-the-value rule (applies everywhere in this skill)

The skill answers "what type of data did I share, and in which file", not "what is the value".

- Always report by category and source file, for example: "Payment-card details found in `insurance/acme-renewal-invoice-2023.pdf` (confidence: high)."
- Never quote, return, display or save the value of a passport or national ID number, card number, security code, bank account number, password or security answer.
- Partial values count too: no "card ending 4821", no first or last digits, no masked fragments.
- If a value seems needed to be useful, describe it by category and location only.

This keeps the report safe to show on screen and safe to store on the NAS, while still telling the user exactly where their sensitive data lives.

## Confidence levels

- **High**: the category is clearly present and unambiguous in the file.
- **Medium**: the category is likely present, but the text is partial, low quality or ambiguous.
- **Low**: weak or indirect signal only. Low-confidence findings go to the needs-human-review list, not the exposure list.

Files that could not be read (encrypted, damaged, unsupported, no readable text) also go to the needs-human-review list, with the reason.
