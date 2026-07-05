---
name: sales-brief
description: Surfaces top and bottom sellers, identifies seasonality patterns, and produces a 2-week content brief to push winners and clear slow movers. Accepts optional lookback window of 30, 60, or 90 days.
allowed-tools: Read, WebFetch, Bash
---

> **Organization context — The Stamford Chorale.** This plugin is customized for The Stamford Chorale, Inc., a 501(c)(3) community choral nonprofit in Stamford, CT. The `## Business context` block in session memory holds the full profile. Apply throughout:
> - **People are** patrons, donors, sponsors, and members/singers — never "customers" or "leads."
> - **Income:** ticket sales (Ludus), member dues (ChorusConnection), program-ad sponsorships, individual donations, and grants from municipal and state arts funders. n8n syncs Ludus and ChorusConnection into QuickBooks, so QuickBooks is the system of record.
> - **Year shape:** spring and fall concert runs plus an annual cabaret fundraiser; each grant needs an annual application and report.
> - **Connectors in use:** QuickBooks, PayPal, Stripe, HubSpot (donor/patron CRM), Microsoft 365 (Outlook email/calendar, SharePoint/OneDrive files, Teams chat), Canva. Gmail, Google Workspace, Slack, and Square are NOT used — use the Microsoft 365 equivalents.

Run the sales analysis and content brief. Pull what sold (and what didn't), explain why, and produce a ready-to-use content plan that acts on the data.

Parse arguments:
- `--lookback` (default: `30d`) — `30d`, `60d`, or `90d` lookback window

## Step 1 — Sales breakdown

Using the `content-strategy` skill workflow for sales analysis:

1. Pull PayPal transactions for the lookback period grouped by item/service/SKU.
2. Pull QuickBooks revenue by product/service category.
3. Rank products by: total revenue, unit volume, and margin (if available in QB).
4. Calculate each product's share of total revenue vs. prior equivalent period.

Top sellers: products that grew share or maintained top-3 rank.
Bottom sellers: products with declining volume or below 5% of revenue.

## Step 2 — Seasonality check

1. Compare current period to same period in prior year (if QB history available).
2. Flag any items with a seasonal pattern (e.g., spikes in Q4, slow summers).
3. Note any new products with insufficient history to detect seasonality.

## Step 3 — Why analysis

For each top and bottom seller, explain the likely driver:
- Price change, promo, new channel, seasonal demand, competitor move
- Cross-reference with HubSpot campaign activity for the period
- Note where attribution is inferred vs. confirmed

## Step 4 — 2-week content brief

Produce a ready-to-use content brief:

```
2-Week Content Brief — {date range}

PUSH THESE (winners)
• {product}: {suggested angle} — {channel: email|social|both}
• {product}: {suggested angle} — {channel}

CLEAR THESE (slow movers)
• {product}: {promo angle or bundle suggestion} — {channel}

CONTENT CALENDAR
Week 1:
  Mon: {post/email concept}
  Wed: {post/email concept}
  Fri: {post/email concept}
Week 2:
  Mon: {post/email concept}
  Wed: {post/email concept}
  Fri: {post/email concept}
```

## Connector failures

If both QuickBooks and PayPal are unreachable, stop — sales analysis requires at least one revenue source. If only one is connected, run from that source and note "QuickBooks not connected — revenue data from PayPal only" (or vice versa). If HubSpot is missing, skip campaign cross-reference in the "why analysis" and note it.

## Approval gates

- **Never auto-schedule or publish content.** The brief is for owner review only.
- **Never create Canva assets automatically** — offer to generate them after owner approves the brief.

## Output

Present the sales analysis, then the content brief. Ask the owner if they'd like to generate Canva assets for any of the planned posts.
