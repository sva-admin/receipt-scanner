---
name: receipt-scanner
description: Turn a pile of photographed or scanned receipts into one auditable expense spreadsheet. Extracts date, vendor, total, tax, category, payment method and currency, flags anything unreadable instead of guessing, appends to an existing ledger, and totals by category and month. Trigger on "scan my receipts", "receipts to spreadsheet", "expense spreadsheet", "sort out my receipts", "categorize my spending", "log these receipts", or when the user points at a folder of receipt photos or drops receipt images into the chat.
---

# Receipt Scanner

A month of paper becomes one spreadsheet a bookkeeper can check line by line.

## Inputs

- A folder path, or receipt images dropped straight into the conversation. Accept JPG, PNG, HEIC, PDF, and multi-page PDFs where one page is one receipt.
- Optional: the columns the user wants, their category list, their home currency, and which period is being closed.

## Objective

One ledger where every row traces back to a specific file, and nothing is invented. The user should be able to pick any row, open the photo it came from, and see the same numbers. A blank the user fills in themselves beats a confident wrong number, every time.

## Ask first, once

Before reading a single image, confirm three things in one short message: the columns, the category list, and the home currency. Then stop asking.

- Default columns when the user has no preference: `Date`, `Vendor`, `Total`, `Tax`, `Category`, `Payment method`, `Currency`, `Source file`, `Confidence`, `Notes`.
- Default categories: read the pile first, propose a set that fits what is actually in it, and get a yes. Reuse that set every month after.
- On later runs, do not re-ask anything. Read the header row of the existing ledger and match it exactly.

## Extract, per receipt

- **Date**: the transaction date, not a print or reprint date. Normalize to `YYYY-MM-DD`. A two-digit year on a faded slip is a `review` row, not a guess.
- **Vendor**: the name as printed, minus branch codes, till numbers, and cashier names. Spell it identically every time or the grouping falls apart.
- **Total**: the amount actually paid, after discounts, including tax. Not the subtotal, not the pre-tip line.
- **Tax**: only when it is printed as its own line. Never derive it from a rate you assume.
- **Category**: from the approved list only. If nothing fits, use `uncategorized` and say so in the summary rather than inventing a category mid-run.
- **Payment method**: only when printed. Card last four digits, cash, transfer.
- **Currency**: ISO code. Foreign-currency receipts keep the original amount and code in their own columns; the converted figure goes in a separate column with the rate and rate date in `Notes`.
- **Source file**: the exact filename. This column is what makes the sheet auditable, so it is never optional.

## The trust rules

These are the point of the skill. Break one and the spreadsheet is worth less than the shoebox.

- **Never guess a number.** If a digit is unreadable, leave the cell blank, set `Confidence` to `review`, and write what was unclear in `Notes`: "total smudged", "date cut off at fold", "thermal print faded".
- **Never invent a vendor** from a logo you half recognize or a phone number you cannot read.
- **Confidence is per row**, either `ok` or `review`. Only `ok` rows count toward totals. Report `review` rows separately so no total is ever quietly wrong.
- **Append, never overwrite.** If a ledger exists at the target path, add rows under the existing header and leave old rows alone.
- **Catch duplicates before writing.** Same vendor, same date, same total means the pile got photographed twice: flag it, do not double count it. Two photos of the front and back of one receipt are one row.
- **Do not touch the originals.** No moving, renaming, or deleting the user's photos unless they ask for it.

## Where output goes

Default root is `receipts/` in the current folder. If the user keeps their books somewhere specific, write there instead and keep the same two files. Confirm the root once, then stop asking.

1. **Ledger** at `receipts/expenses.csv`, append-only, one row per receipt, columns exactly as agreed.
2. **Summary** at `receipts/summary-YYYY-MM.md`, containing: total for the period, total per category, total per month, top five vendors by spend, tax total, and a **Needs review** list naming every flagged file and the reason it was flagged.
3. **Report back in chat**: the period total, the three largest categories, and the count of flagged rows with their filenames. The table stays in the file, not in the chat.

If the user wants an `.xlsx`, write the CSV first and convert from it. The CSV stays the source of truth because it opens anywhere and survives everything.

## Verify before reporting

Run these checks and only then report. If one fails, fix it before speaking.

- Row count equals the number of receipt files read, minus the duplicates you named out loud.
- Spot check three rows at random against their source images, and say in chat which three you checked. This is what turns the sheet from hopeful into trusted.
- Category totals for `ok` rows add up to the period total.
- No row has an amount without a currency, and no row has tax larger than its total.
- Every `review` row names both a file and a reason. "Unclear" on its own is not a reason.

## Recurring use

This works well on a schedule, and weekly beats monthly because the photos are still fresh enough to sort out anything odd:

```
Every Friday at 5pm, process any new receipts in ~/receipts, append them to the ledger, and message me only the flagged rows.
```

## When to use it

- Three receipts on a table: just read them. No skill needed.
- A folder, a closed month, a tax deadline, or anything a second person will audit: use this.

## Style and anti-patterns

- The flagged list goes first, not buried under a cheerful total. Blanks are the feature.
- Do not tidy vendor names into something prettier than what is printed on the paper.
- Do not reclassify last month's rows to make this month's chart look neater.
- Currency conversion is an extra column, never an overwrite of the original amount.
- Do not offer tax advice. Sort the numbers, name what is unclear, and leave the ruling to their accountant.
