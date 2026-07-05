# TSC Plugin — operating workflows for The Stamford Chorale

Pre-built operating workflows for [Cowork](https://claude.com/product/cowork), Anthropic's agentic desktop application — also works in Claude Code. This copy has been tailored for **The Stamford Chorale, Inc.**, a 501(c)(3) community choral nonprofit in Stamford, CT. Install it once and you get a set of building-block skills, ready-to-use workflows, and a router that understands plain English.

You don't need to memorize anything. Just tell Claude what you need — "are we covered through the spring concert," "draft the sponsor thank-yous," "what's due for the state arts grant?" — and it figures out the right workflow and walks you through it. Every workflow pauses before taking action, so nothing happens without your say-so.

> **Important**: This plugin assists with day-to-day chorale operations but does not provide financial, tax, legal, or HR advice. All outputs should be reviewed by you (and where appropriate, a qualified professional — accountant, attorney) before use.

## Your organization at a glance

- **Who:** The Stamford Chorale, Inc. — a volunteer community chorus and registered 501(c)(3) nonprofit.
- **People you work with:** patrons, donors, sponsors, and members/singers — not "customers" or "leads."
- **Money in:** ticket sales, member dues, program-ad sponsorships, individual donations, and grants.
- **Year shape:** two main concert runs a year (spring and fall), plus an annual cabaret fundraiser, with occasional ad-hoc performances.
- **Grants:** several funders (municipal and state arts funders) that each require an annual application and report.

## Your tool stack

This copy is wired for the tools the Chorale actually uses:

- **QuickBooks** — the financial system of record (cash, deposits, bills, grant income, P&L, reconciliation, 1099s).
- **PayPal** and **Stripe** — online payments for tickets and donations.
- **HubSpot** — the donor/patron/sponsor CRM (contacts, outreach, campaigns).
- **Microsoft 365** — Outlook (email & calendar), SharePoint/OneDrive (files), and Teams (chat). *Used instead of Gmail / Google Workspace / Slack.*
- **Canva** — concert posters, program design, and social assets.

**Connected outside this plugin:**

- **Ludus** — your ticketing platform (box office and online ticket sales).
- **ChorusConnection** — member management, including member dues/payments.
- **n8n** — automations that sync Ludus and ChorusConnection data into QuickBooks, so QuickBooks stays the single source of truth.

When a workflow needs ticket-level or member-level detail, Claude will look in QuickBooks first (where the n8n syncs land) and ask you for a Ludus or ChorusConnection export if it needs more granularity.

## Getting started

Say **"set me up"** to run the `smb-onboard` skill. Because this copy is pre-customized, onboarding will simply confirm the Chorale's profile, make sure your tools are connected, and set a weekly check-in rhythm — no cold interview required.

## How it works

Three layers working together:

1. **Skills** — the building blocks. Each skill knows how to do one thing really well (forecast cash, draft sponsor outreach, write a board-ready review).

2. **Commands** — the workflows. Commands chain skills together into multi-step recipes with checkpoints where you approve before anything happens.

3. **The Router** — the front door. You talk to Claude in plain English. The router listens, figures out which workflow fits, and gets you there. You never need to memorize a command name.

## Commands

Commands are workflows that chain skills together. Each one pauses at checkpoints for your approval before taking action.

### Money & finance

| Command | What it does | Just say... | Skills used | Required | Optional |
|---|---|---|---|---|---|
| `/plan-payroll` | Cash forecast + chase on outstanding sponsor/ad invoices and pledged grant funds so you know the season is covered. | "are we covered for the spring concert", "cash is tight", "who still owes us" | cash-flow-snapshot, invoice-chase | QuickBooks | PayPal, Stripe, Outlook |
| `/month-heads-up` | 30-day cash outlook with early risk flags. | "what does next month look like", "cash forecast", "runway" | cash-flow-snapshot | QuickBooks | PayPal |
| `/close-month` | Month-end close: reconcile QuickBooks against PayPal/Stripe (and the Ludus/ChorusConnection syncs), flag gaps, write a plain-English summary, export a packet. | "close the books", "month-end", "reconcile" | month-end-prep | QuickBooks | PayPal, Stripe |
| `/tax-prep` | Year-end materials for your accountant: 1099-NEC prep for paid contractors (music director, accompanist, soloists, designers) and a clean handoff packet. | "tax stuff", "1099s", "what the accountant needs" | tax-season-organizer | QuickBooks | PayPal, Stripe |

### Fundraising & outreach

| Command | What it does | Just say... | Skills used | Required | Optional |
|---|---|---|---|---|---|
| `/call-list` | Top donors/sponsors to reach out to this week with talking points and calendar blocks. | "who should I thank", "sponsor follow-ups", "donor outreach" | lead-triage | HubSpot | Outlook, Outlook Calendar |
| `/run-campaign` | End-to-end concert or appeal campaign: sales/giving analysis → content brief → Canva assets → HubSpot send. | "promote the spring concert", "year-end appeal", "sell more program ads" | content-strategy, canva-creator, lead-triage | HubSpot, Canva | QuickBooks, PayPal |
| `/sales-brief` | What's drawing ticket sales and giving, plus a 2-week promotion brief. | "how are ticket sales", "what should we promote" | content-strategy | QuickBooks or PayPal | HubSpot |

### Patrons & operations

| Command | What it does | Just say... | Skills used | Required | Optional |
|---|---|---|---|---|---|
| `/customer-pulse-check` | Patron and member feedback themes with response templates. | "what are patrons saying", "concert feedback", "member concerns" | customer-pulse, ticket-deflector | PayPal or HubSpot | -- |
| `/handle-complaint` | End-to-end issue resolution: pull context, draft a response, suggest a fix. | "a patron is upset", "ticket problem", "refund request" | ticket-deflector, customer-pulse | -- (works with pasted text) | Outlook, HubSpot, PayPal |
| `/crm-cleanup` | HubSpot hygiene: stale records, duplicates, missing fields — fixes what you approve. | "clean up the donor list", "HubSpot is a mess", "dedupe contacts" | crm-maintenance | HubSpot | -- |
| `/review-contract` | Plain-English review of venue, performer, or vendor agreements with red flags and severity ratings. | "review this contract", "venue agreement", "should I sign this" | contract-review | -- (works with file upload) | -- |

### Board & planning

| Command | What it does | Just say... | Skills used | Required | Optional |
|---|---|---|---|---|---|
| `/monday-brief` | Weekly briefing: cash, ticket/giving trend, outreach, week ahead, top 3 to-dos. | "weekly check-in", "what's on my plate", "start of week" | business-pulse | -- (degrades gracefully) | QuickBooks, PayPal, HubSpot, Outlook Calendar, Outlook |
| `/friday-brief` | End-of-week pulse: income vs last week, wins, and things to watch. | "end of week", "how'd we do", "Friday recap" | business-pulse | PayPal or HubSpot | -- |
| `/quarterly-review` | Board-ready review: income trend, fund/grant health, donor/patron health, opportunities, risks. | "board report", "treasurer's report", "quarterly review" | business-pulse | QuickBooks | PayPal, HubSpot |

## Skills

Skills are the atomic building blocks. Each one does one thing well.

### Money & finance

| Skill | What it does | Just say... | Required | Optional |
|---|---|---|---|---|
| **cash-flow-snapshot** | 30/60/90-day cash forecast with confidence bands and named risk flags. Chat summary + XLSX. | "forecast our cash", "are we covered for the concert", "runway" | QuickBooks, PayPal, or Stripe (any one) | Others as secondary sources |
| **invoice-chase** | Drafts reminders for outstanding program-ad/sponsor invoices and pledged-but-unpaid commitments, matched to each contact's history and tone. | "who still owes us", "unpaid sponsor invoices", "follow up on pledges" | QuickBooks | PayPal, Stripe, Outlook |
| **month-end-prep** | Month-end close: reconciles QuickBooks against PayPal/Stripe and the Ludus/ChorusConnection syncs, flags gaps, writes a plain-English summary, exports a packet. | "close the month", "reconcile", "why did income change" | QuickBooks | PayPal, Stripe |
| **tax-season-organizer** | Year-end 1099-NEC prep for paid contractors plus an accountant handoff packet. (Also supports estimated-tax math if ever needed.) | "1099s", "year-end tax prep", "what the accountant needs" | QuickBooks | PayPal, Stripe |

### Fundraising & marketing

| Skill | What it does | Just say... | Required | Optional |
|---|---|---|---|---|
| **lead-triage** | Scores HubSpot donors/sponsors/prospects by engagement, fit, and timeliness to produce a ranked outreach list with talking points. | "who to thank first", "sponsor prospects", "donor outreach" | HubSpot | Outlook, Outlook Calendar |
| **content-strategy** | Analyzes ticket/giving data to find what's working and what's lagging, produces a prioritized 30-day promotion brief. | "what should we post", "promotion plan", "how are ticket sales" | QuickBooks or PayPal | HubSpot |
| **canva-creator** | Takes a content brief and executes the full campaign: posting calendar, Canva assets (posters, social, program ads), caption copy, HubSpot staging. | "make the concert posts", "design the program ad", "turn this into a campaign" | Canva, HubSpot | -- |

### Patrons & operations

| Skill | What it does | Just say... | Required | Optional |
|---|---|---|---|---|
| **customer-pulse** | Aggregates disputes, feedback, email sentiment, and reviews into a themes report with a "do these three things this week" list. | "how are patrons feeling", "concert feedback", "what people are saying" | -- (degrades gracefully) | PayPal, HubSpot, Outlook |
| **ticket-deflector** | Reads a patron email, pulls order/refund status, drafts a tone-matched reply. Can issue PayPal refunds with approval. | "answer this patron", "where's my ticket", "refund request" | PayPal, HubSpot, Outlook | -- |
| **crm-maintenance** | Keeps HubSpot current: creates/updates donor and sponsor records, logs calls and notes, flags stale records. | "update the donor list", "log a call", "add context to a sponsor" | HubSpot | Outlook, Outlook Calendar |
| **contract-review** | Plain-English review of venue, performer, and vendor agreements with risk flags, severity ratings, and a marked-up redline DOCX. | "review this contract", "what am I signing", "check the venue terms" | -- (works with file upload) | Outlook |

### Hiring

| Skill | What it does | Just say... | Required | Optional |
|---|---|---|---|---|
| **job-post-builder** | Builds a hiring packet for paid roles (music director, accompanist, section leaders): posting, structured interview guide with scoring rubric, and offer letter template. | "help me hire", "post for an accompanist", "interview questions", "offer letter" | -- (works standalone) | OneDrive |

### Board intelligence & onboarding

| Skill | What it does | Just say... | Required | Optional |
|---|---|---|---|---|
| **business-pulse** | One-page snapshot: cash, ticket/giving trend, outreach, commitments, watch-list, and the single most important thing needing attention today. | "weekly check-in", "how are we doing", "catch me up" | -- (degrades gracefully) | QuickBooks, PayPal, HubSpot, Outlook Calendar, Outlook |
| **smb-onboard** | Confirms the Chorale's profile, makes sure your tools are connected, runs a demo, and sets a weekly check-in cadence. | "set me up", "setup", "get started", "what can you do" | -- | All connectors |

## Customizing further

These workflows are already tailored to the Chorale, but you can keep refining them:

- **Add detail** — Drop specific funders, deadlines, venues, or recurring vendors into skill files so Claude knows your world even better.
- **Adjust thresholds** — Tune the alert thresholds in `business-pulse` and `cash-flow-snapshot` to match your scale (annual operating budget is roughly $50–60K).
- **Swap connectors** — If you adopt or drop a tool, update `.mcp.json` and the affected skills.
