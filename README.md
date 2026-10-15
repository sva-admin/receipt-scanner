<p align="center"><a href="https://loop.sv-academy.org"><img src="https://raw.githubusercontent.com/sva-admin/claude-skills/main/assets/sv-academy-512.png" width="96" alt="Silicon Valley Academy" /></a></p>

# receipt-scanner

Photograph a month of receipts. Get one spreadsheet you can actually trust.

> **Transparency**
>
> A clean-room SV Academy original. Inspired by an unlicensed community `receipt-scanner` skill. We read that skill only to understand the job it does, then wrote every instruction here from scratch. Full credit below.

## Learn it free, step by step

[Receipts to spreadsheet](https://loop.sv-academy.org/articles/receipts-to-spreadsheet)

## Try it

Paste this into Claude:

```
Install the skill from https://github.com/sva-admin/receipt-scanner, then turn the receipt photos in ~/receipts/august into one spreadsheet with date, vendor, total, tax, and category.
```

## Use it

```
Scan my receipts folder and give me a spreadsheet grouped by category
Here are three months of receipts. Flag anything you can't read instead of guessing.
Split these into fuel and materials, tax in its own column, monthly total for each
These are in two currencies. Convert to THB, keep the original in its own column.
```

## What you get

An append-only ledger at `receipts/expenses.csv`, one row per receipt, with a `Source file` column so any number can be checked against the photo it came from in about ten seconds. Alongside it, a monthly summary with totals per category, top vendors, tax total, and a **Needs review** list.

The reason this is worth doing at all is the last part. A faded till roll, handwriting, or a folded corner produces a blank cell marked `review` with a reason next to it, never a plausible-looking number. Only confident rows count toward totals, so the total is either right or clearly incomplete, and never quietly wrong. It also spot checks three rows against the original images before it reports, and tells you which three.

Runs fine on a schedule. Weekly is better than monthly, because you can still remember what the smudged one was.

## Credit

Inspired by the `receipt-scanner` skill bundled in the community anthropic-skills collection. That copy ships with no license file and no named author, so there is no canonical upstream we can link. **This is an independent SV Academy implementation**: the job is the same, every sentence in `SKILL.md` is ours, and the SV Academy text is MIT licensed (see [LICENSE](LICENSE)).

If you wrote the original and want a direct link, a different credit line, or this repo taken down, open an issue and we will act on it.

## More from Silicon Valley Academy

- All of our published skills: [github.com/sva-admin/claude-skills](https://github.com/sva-admin/claude-skills)
- Courses and free lessons: [sv-academy.org](https://sv-academy.org)
- Software and AI built for your business: [svinnovation.com](https://svinnovation.com), Silicon Valley Innovation, the company behind SV Academy
