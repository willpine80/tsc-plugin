---
name: plan-payroll
description: Forecasts cash, ranks overdue invoices, and stages PayPal reminders so the owner can confidently run payroll. Accepts optional horizon and payroll-date arguments.
allowed-tools: Read, WebFetch, Bash
---

> **Organization context — The Stamford Chorale.** This plugin is customized for The Stamford Chorale, Inc., a 501(c)(3) community choral nonprofit in Stamford, CT. The `## Business context` block in session memory holds the full profile. Apply throughout:
> - **People are** patrons, donors, sponsors, and members/singers — never "customers" or "leads."
> - **Income:** ticket sales (Ludus), member dues (ChorusConnection), program-ad sponsorships, individual donations, and grants from municipal and state arts funders. n8n syncs Ludus and ChorusConnection into QuickBooks, so QuickBooks is the system of record.
> - **Year shape:** spring and fall concert runs plus an annual cabaret fundraiser; each grant needs an annual application and report.
> - **Connectors in use:** QuickBooks, PayPal, Stripe, HubSpot (donor/patron CRM), Microsoft 365 (Outlook email/calendar, SharePoint/OneDrive files, Teams chat), Canva. Gmail, Google Workspace, Slack, and Square are NOT used — use the Microsoft 365 equivalents.

Run the payroll-confidence pipeline by chaining two skills. The owner approves at each handoff — never send a reminder or commit a forecast without explicit confirmation.

Parse arguments:
- `--horizon` (default `30`) — forecast window in days (30, 60, or 90)
- `--payroll-date` (optional) — the date payroll runs; defaults to next Friday

## Step 1 — Cash forecast (cash-flow-snapshot)

Trigger the `cash-flow-snapshot` skill workflow:
1. Pull AR, AP, and historical cash timing from QuickBooks, PayPal, Stripe, or Stripe (whichever are connected). Fall back to CSV upload if no connector is live.
2. Layer in known fixed costs (rent, payroll, recurring vendor charges).
3. Produce a 30/60/90-day forecast (use the requested `--horizon`) with percentage-variance confidence bands.
4. Flag named risks — e.g., "payroll on May 15 lands $4,200 below your fixed-cost floor at the median forecast."
5. Deliver chat summary + downloadable XLSX.
6. Present to the owner. Wait for explicit "okay, see what we can collect" before Step 2.

If the forecast shows payroll is comfortably covered, ask the owner whether they still want to chase overdue invoices or stop here.

## Step 2 — Overdue collection (invoice-chase)

After Step 1 approval, trigger the `invoice-chase` skill workflow:
1. Pull overdue invoices from QuickBooks and PayPal.
2. Rank by amount × days-late × customer payment history.
3. For each, draft a reminder matched to tone (gentle for good customers, firm for repeat late payers).
4. PayPal-issued invoices queue as PayPal-send drafts; non-PayPal invoices queue as Mail drafts.
5. Present the ranked list with drafted reminders. Show the projected cash impact if a top-N subset gets paid within the horizon — does that close the payroll gap from Step 1?
6. Wait for explicit "send these" per reminder (or batch approval) before pushing.

## Approval gates (must hold)

- Never send a reminder without owner approval — drafts only until "send" is given.
- Never commit a forecast as authoritative without owner sign-off.
- If a connector is unreachable (QuickBooks, PayPal, Mail), stop, report which connector failed, and ask whether to retry, fall back to CSV, or abort.

## Output

End the run with a one-paragraph recap: forecast verdict (covered / gap / risk), reminders sent and to whom, projected new cash position if reminders convert.
