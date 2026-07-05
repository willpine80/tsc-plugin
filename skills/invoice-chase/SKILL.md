---
name: invoice-chase
version: 0.2.0
description: >
  Drafts overdue-invoice reminder emails from QuickBooks and PayPal data,
  matched to each customer's payment history and tone (gentle for good customers,
  firm for repeat late payers). Sends via PayPal with owner approval;
  non-PayPal invoices queue as mail drafts. Use when the user asks
  "who owes me money," mentions overdue invoices, or wants to follow up
  on unpaid invoices.
---

> **Organization context — The Stamford Chorale.** This plugin is customized for The Stamford Chorale, Inc., a 501(c)(3) community choral nonprofit in Stamford, CT. The `## Business context` block in session memory holds the full profile. Apply throughout:
> - **People are** patrons, donors, sponsors, and members/singers — never "customers" or "leads."
> - **Income:** ticket sales (Ludus), member dues (ChorusConnection), program-ad sponsorships, individual donations, and grants from municipal and state arts funders. n8n syncs Ludus and ChorusConnection into QuickBooks, so QuickBooks is the system of record.
> - **Year shape:** spring and fall concert runs plus an annual cabaret fundraiser; each grant needs an annual application and report.
> - **Connectors in use:** QuickBooks, PayPal, Stripe, HubSpot (donor/patron CRM), Microsoft 365 (Outlook email/calendar, SharePoint/OneDrive files, Teams chat), Canva. Gmail, Google Workspace, Slack, and Square are NOT used — use the Microsoft 365 equivalents.

# Invoice Chase

## Quick start

Pull the AR aging report, score each customer by payment history, draft a tone-matched reminder for each overdue invoice, and present them to the owner. Nothing sends until the owner says so.

```
User: "who owes me money"
→ Pull AR aging from QuickBooks
→ Cross-reference PayPal settlements (last 14 days)
→ Score each customer: good-payer / occasionally-late / repeat-late
→ Draft tone-matched reminders
→ Show summary table + drafts. Wait for "send these."
```

## Setup (first run only)

Ask the owner two questions before running for the first time:

1. **Mail connector**: "Do you use Outlook or Apple Mail for drafts?" — store the answer; use it for all non-PayPal draft queuing.
2. **Stripe**: "Do you use Stripe for invoicing? I can include Stripe invoices in the overdue sweep." — if yes, pull Stripe overdue invoices alongside QuickBooks.

Do not ask again on subsequent runs.

## Workflow

1. **Pull overdue receivables.** Query QuickBooks AR aging for all invoices more than 1 day past due. If Stripe is enabled (owner confirmed at setup), also pull Stripe overdue invoices.

2. **Cross-reference payment history.** For each overdue customer, query PayPal for settled transactions using these parameters:
   - `transaction_status: S` (settled only — filters out pending and denied transactions that inflate result size and increase rate-limit risk)
   - Date window: **last 7 days** ending today (not 14 or 30 — wider windows are the primary cause of PayPal 429 rate limit errors)

   **If PayPal returns a 429 rate limit error:**
   - Retry once immediately with a **3-day window** instead.
   - If the retry also returns 429, skip the PayPal cross-reference entirely for this run. Flag all customers in the batch as "PayPal unavailable — verify manually" in the summary table. Proceed to scoring using QuickBooks history only. Do not silently drop the caveat.

   If a customer shows a settled payment within the query window, flag as "possibly paid — verify" and exclude from the draft queue.

3. **Score each customer.** Read [reference/tone-matching.md](reference/tone-matching.md) for scoring logic. Result: `good-payer`, `occasionally-late`, or `repeat-late`.

4. **Draft reminder emails.** One email per customer — consolidate multiple overdue invoices into one email. Match tone to score. See [reference/examples/gentle-reminder.md](reference/examples/gentle-reminder.md) and [reference/examples/firm-reminder.md](reference/examples/firm-reminder.md).

5. **Present drafts to owner.** Show a summary table first:

   | Customer | Amount Due | Days Late | Tone | Send via |
   |---|---|---|---|---|
   | Acme Corp | $1,200 | 18 days | Gentle | PayPal |
   | Smith LLC | $450 | 47 days | Firm | Outlook draft |

   Then show each draft email in full. Wait for owner to say "send these" or approve individually.

6. **Send or queue — only after approval.**
   - PayPal invoices: send the reminder via PayPal.
   - Non-PayPal invoices: queue as a draft in the owner's configured mail app.
   - Never send without explicit approval.

7. **Report what happened.** List what was sent, what was queued as draft, and what was flagged (possibly paid, excluded).

## Approval gates

- **Never send or queue a draft without explicit owner approval.** Present all drafts first; wait for the go-ahead.
- **Never include a customer who paid in the last 14 days.** Flag as "possibly paid — verify" instead.
- **Never send to a customer not in the QuickBooks AR report** (or Stripe, if enabled). No reminders from memory alone.
- **One approval covers one batch.** Adding a customer or changing a draft after approval starts a new round.

## Reference

- [reference/tone-matching.md](reference/tone-matching.md) — scoring logic, tone guidelines, subject line formulas
- [reference/gotchas.md](reference/gotchas.md) — known failure modes
- [reference/examples/gentle-reminder.md](reference/examples/gentle-reminder.md) — good-payer email example
- [reference/examples/firm-reminder.md](reference/examples/firm-reminder.md) — repeat-late-payer email example
