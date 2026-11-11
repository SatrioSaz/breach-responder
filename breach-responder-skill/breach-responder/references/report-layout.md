# Report layout

Use this layout for the report shown in chat in step 4 and saved in step 5. Fill it only with real results from this run. If a section has no data, write "None found" instead of leaving it out. Never include a card, ID, account number or password value, in full or in part.

```markdown
# Breach exposure report: {company}

Date: {today}
Folders searched: {folders}
Time range: {time range, or "all dates"}
Other names searched: {aliases, or "none given"}

## Summary
- Linked files found: N (by file name: a, by document content: b, by screenshot text: c; some files were found more than once)
- Personal data types confirmed: {categories}
- Highest-risk type found: {category, or "none found"}
- Files that need a human look: M

## Personal data found
| Data type | Confidence | Found in |
|---|---|---|
| Payment-card details | High | insurance/acme-renewal-invoice-2023.pdf |

## Needs human review
| File | Reason |
|---|---|
| Screenshots/acme/IMG_2291.jpg | Text too unclear to judge |

## Action checklist (highest risk first)
1. {action} ({data type} · {source file})

## Evidence files
| File | Type | Date | Found by |
|---|---|---|---|

## Not checked
- Steps that failed or were cancelled, with the reason.

This report is based only on files stored on your NAS. It cannot confirm whether your data was part of the breach. Check the company's official breach notice for that.
```
