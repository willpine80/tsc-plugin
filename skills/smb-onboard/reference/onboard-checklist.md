# Onboard checklist

## This copy is pre-customized for The Stamford Chorale

The plugin already knows this organization. **Do not run a cold interview.** Instead, show the pre-filled profile below, ask the user to confirm or correct it, then write it to memory. Only fall back to the five questions if the user says the profile is wrong and wants to start over.

### Pre-filled profile (confirm, don't re-ask)

- **Organization:** The Stamford Chorale, Inc. — a 501(c)(3) community choral nonprofit in Stamford, CT.
- **People served:** patrons, donors, sponsors, members/singers.
- **Income streams:** ticket sales (Ludus), member dues (ChorusConnection), program-ad sponsorships, individual donations, grants from municipal and state arts funders.
- **Year shape:** spring and fall concert runs, an annual cabaret fundraiser, occasional ad-hoc performances. Each grant has an annual application and report.
- **Tools in use:** QuickBooks (system of record), PayPal & Stripe (payments), HubSpot (donor/patron CRM), Microsoft 365 (Outlook, SharePoint/OneDrive, Teams), Canva. Ludus and ChorusConnection feed QuickBooks via n8n.
- **Typical headaches:** keeping cash visible across seasons, grant application/report deadlines, sponsor and donor follow-up, month-end reconciliation across PayPal/Ludus/ChorusConnection.

## The five interview questions (fallback only — use if the profile above is wrong)

Ask one at a time. Wait for the full answer before moving on. One follow-up is fine if an answer is vague; do not drill further.

1. **Organization and mission.** "Tell me about the Chorale in a sentence — what should I keep in mind?"
2. **People involved.** "Roughly how many active members and board members, and who handles the books?"
3. **Top three headaches.** "What are your three biggest headaches right now — the things that eat your time?"
4. **Tools already in use.** "Which tools do you use day-to-day? QuickBooks, PayPal, HubSpot, Microsoft 365, Canva, Ludus, ChorusConnection…"
5. **Preferred cadence.** "How would you like me to check in — weekly, or only when you ask?"

If the user is short on time, confirm the pre-filled profile and move straight to setting the cadence.

---

## Connector priority matrix

Map the user's stated headache to the best two connectors to link first.

| Primary headache | First connector | Second connector | Prove-value recipe |
|---|---|---|---|
| Cash flow / season coverage | QuickBooks | PayPal or Stripe | `cash-flow-snapshot` |
| Donor / sponsor follow-up | HubSpot | Outlook (Microsoft 365) | `crm-maintenance` (read-only demo) |
| Concert promotion | HubSpot | Canva | `content-strategy` |
| Grant / board deadlines | Outlook Calendar (Microsoft 365) | QuickBooks | `business-pulse` |
| Month-end reconciliation | QuickBooks | PayPal or Stripe | `month-end-prep` |
| General / unsure | QuickBooks | HubSpot | `cash-flow-snapshot` |

All email, calendar, files, and chat run through the **Microsoft 365** connector (Outlook, SharePoint/OneDrive, Teams) — never Gmail, Google, or Slack. If the user names a connector not in this table, add it as the second connector and use `business-pulse` as the recipe.

---

## Recipe selection

Run the prove-value recipe immediately after the **first** connector is live — do not wait for the second. If connectors are already active at session start, run the matched recipe for the user's primary headache before confirming the profile. Priority order:

1. QuickBooks → `cash-flow-snapshot`
2. HubSpot → `crm-maintenance` (log-a-note demo, read-only)
3. PayPal or Stripe → recent income snapshot (tickets + donations)
4. Microsoft 365 (Outlook) → surface top 3 time-sensitive threads (e.g. grant deadlines, sponsor replies)
5. Canva → `content-strategy` brief for the next concert

**QuickBooks profile_info_required:** If QuickBooks returns a `profile_info_required` status (missing business_name or industry), use the `quickbooks-profile-info-update` tool with the owner's business name from interview question 1 before running `cash-flow-snapshot`. Do not skip the recipe — collect the missing info first.

---

## Organization profile — storage format

Write this block to the Cowork session memory directory under the heading `## Business context`. Every other skill reads this section by heading match. Do not rename the heading or change the field names. Below is the pre-filled block for The Stamford Chorale — confirm it with the user (update only what changed), set the `Onboarded` date, then save it verbatim.

```markdown
## Business context

- **Business:** The Stamford Chorale, Inc. — 501(c)(3) community choral nonprofit in Stamford, CT.
- **People served:** patrons, donors, sponsors, members/singers (never "customers" or "leads").
- **Income streams:** ticket sales (Ludus), member dues (ChorusConnection), program-ad sponsorships, individual donations, grants from municipal and state arts funders.
- **Year shape:** spring & fall concert runs + annual cabaret fundraiser; each grant has an annual application and report.
- **Connected tools:** QuickBooks (system of record), PayPal, Stripe, HubSpot (donor/patron CRM), Microsoft 365 (Outlook/SharePoint/OneDrive/Teams), Canva. Ludus & ChorusConnection sync into QuickBooks via n8n. Not used: Gmail, Google Workspace, Slack, Square.
- **Top headaches:** cash visibility across seasons · grant deadlines & reporting · sponsor/donor follow-up
- **Weekly cadence:** "weekly check-in" (default Monday)
- **Onboarded:** <YYYY-MM-DD>
```

If a memory file already exists, append or update only the `## Business context` section. Do not touch other content.
