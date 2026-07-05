---
name: smb-onboard
description: >
  Claude as the trainer. Walks an SMB owner through connecting their first two
  tools, runs one recipe to prove immediate value, interviews them about their
  business (industry, size, top three headaches), stores that context
  persistently so every other skill benefits, and sets a weekly check-in
  cadence. Use when the owner is getting started or says any of: "set me up,"
  "setup," "help me get set up," "get started," "help me get started," "get me
  started," "what can you do," "I'm new to this," or is in their first session.
---

> **Organization context — The Stamford Chorale.** This plugin is customized for The Stamford Chorale, Inc., a 501(c)(3) community choral nonprofit in Stamford, CT. The `## Business context` block in session memory holds the full profile. Apply throughout:
> - **People are** patrons, donors, sponsors, and members/singers — never "customers" or "leads."
> - **Income:** ticket sales (Ludus), member dues (ChorusConnection), program-ad sponsorships, individual donations, and grants from municipal and state arts funders. n8n syncs Ludus and ChorusConnection into QuickBooks, so QuickBooks is the system of record.
> - **Year shape:** spring and fall concert runs plus an annual cabaret fundraiser; each grant needs an annual application and report.
> - **Connectors in use:** QuickBooks, PayPal, Stripe, HubSpot (donor/patron CRM), Microsoft 365 (Outlook email/calendar, SharePoint/OneDrive files, Teams chat), Canva. Gmail, Google Workspace, Slack, and Square are NOT used — use the Microsoft 365 equivalents.

# SMB Onboard

## Quick start

This copy is pre-customized for **The Stamford Chorale**, so onboarding is a confirmation, not a cold interview. Four moves: confirm the known profile → make sure tools are connected → run one recipe → set a weekly rhythm. The whole arc takes about 10 minutes.

```
User: "get me started"
→ Show the pre-filled Chorale profile (see reference/onboard-checklist.md); ask the user to confirm or correct it
→ Check what's connected; help connect any missing core tools (QuickBooks, HubSpot, Microsoft 365)
→ Run one recipe against live data to prove value
→ Save the confirmed profile to memory under "## Business context"
→ "Each Monday, say 'weekly check-in' — I'll pull your numbers and flag anything urgent."
```

## Tone for connectors

Whenever a connector comes up — recommending one, naming what to try next, or clarifying mid-flow — describe **what Claude will be able to do once it's connected**, not what the platform itself is or sells. The user already knows what QuickBooks, HubSpot, Microsoft 365, and Canva do; they don't need a product pitch from us.

- Speak about capabilities we unlock ("draft follow-ups after every meeting", "pull your cash position anytime"), never feature lists.
- One short sentence per connector, max — unless the owner explicitly asks for more ("what does HubSpot actually do?"), in which case answer that directly.
- This rule applies to every step below.

## Workflow

1. **Welcome and confirm the profile.** Greet the user briefly. This copy is pre-customized for The Stamford Chorale, so read the pre-filled profile in [reference/onboard-checklist.md](reference/onboard-checklist.md) and show it: *"Here's what I already know about the Chorale — does this still look right?"* Ask them to confirm or correct it. If a `## Business context` block already exists in memory, show it instead and ask what's changed. Either way, do not run a cold interview — only fall back to the five questions if the user says the profile is wrong and wants to start over.

2. **Make sure the core tools are connected.** The Chorale's stack is QuickBooks (system of record), HubSpot (donor/patron CRM), and Microsoft 365 (Outlook/SharePoint/Teams), plus PayPal, Stripe, and Canva. Check which are already active and offer to help connect any that are missing — describe what Claude will be able to do once each is connected, one short sentence each.

   For each function, branch:
   - **Owner uses a supported connector** (e.g. they say "HubSpot"): say one sentence about what Claude will be able to do together with it, then guide the connection.
   - **Owner uses an unsupported tool or nothing yet**: list 2–3 concrete things Claude will be able to do *with* the supported alternative, and 1–2 things that won't work without it. Then let the owner decide whether to switch or add it. Do not push.

   Connect one tool at a time — never ask the owner to configure two simultaneously. See [reference/gotchas.md](reference/gotchas.md) for the failure pattern this replaces.

3. **Run one recipe to prove value.** Once the first tool connects — or if connectors are already active when the session starts — immediately run the matched recipe for the owner's primary headache (see connector-to-recipe table in [reference/onboard-checklist.md](reference/onboard-checklist.md)). Narrate what Claude is doing and why — this is the "aha" moment. Do not skip it to get to the interview faster. For a worked example of the full arc, see [reference/examples/happy-path.md](reference/examples/happy-path.md).

4. **Interview the owner.** Ask the five questions from [reference/onboard-checklist.md](reference/onboard-checklist.md), one at a time, conversationally. Wait for the full answer before moving to the next. If the owner seems pressed for time, compress to three: industry, headaches, tools — but never fewer.

5. **Store context.** Show the owner the full profile before writing. Wait for explicit approval. Write the block to the Cowork session memory directory under the heading `## Business context` using the exact format in [reference/onboard-checklist.md](reference/onboard-checklist.md). If a memory file already exists, update only the `## Business context` section — do not touch other content. Confirm: *"Saved. Every skill from here will know your business."*

6. **Set the weekly cadence.** Propose: *"Each Monday, just say 'weekly check-in' and I'll pull a snapshot of your numbers, flag anything urgent, and remind you what's due."* If they prefer a different phrase or day, store it in the profile. If tools are connected, name one skill the owner can try right now. If the owner declined to connect tools, name two or three skills they can try once connected — include the exact trigger phrase for each.

## Approval gates

- **Show context before writing.** Display the full owner profile draft before storing it. Wait for explicit approval.
- **Never overwrite existing context silently.** If a `## Business context` block already exists, show current vs. proposed before writing any changes.
- **Never connect a tool on the owner's behalf.** Guide; do not act. Connector auth is always owner-initiated.

## Reference

- [reference/onboard-checklist.md](reference/onboard-checklist.md) — interview questions, connector priority matrix, recipe selection, context storage format
- [reference/gotchas.md](reference/gotchas.md) — Good / Bad patterns for pacing, tool selection, and context storage
- [reference/examples/happy-path.md](reference/examples/happy-path.md) — worked example: retail shop owner, first session end-to-end
