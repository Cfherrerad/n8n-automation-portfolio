# Automated Client Onboarding System (n8n)

**Case study:** End-to-end onboarding automation for a digital marketing agency, eliminating manual data entry, task creation, and internal notifications from the moment a client pays until their project is fully active.

> Note: This project was built as part of an advanced automation training program, based on a realistic agency scenario (fictional client names). The workflows, logic, and error-handling shown here reflect production-level design decisions.

---

## The Problem

The agency's operations team was losing **~2 hours per new client** on fully manual onboarding: copying data between spreadsheets, sending individual emails, and creating tasks one by one across three service tiers (Starter, Growth, Premium). This caused data errors, missed steps, and slower client activation.

## The Solution

I designed and built a **4-workflow automation system in n8n** that runs the entire onboarding lifecycle without manual intervention:

### 1️⃣ Welcome & Briefing Automation
Triggers the moment a client's payment is confirmed. Detects the purchased plan (Starter/Growth/Premium) and automatically sends the correct welcome email with a linked Tally intake form — replacing a manual, plan-dependent email process.

### 2️⃣ Operations Engine (core workflow)
Triggers when the client submits their Tally brief. Registers the client in **Airtable via direct HTTP Request calls** (not the native module, to demonstrate raw API integration), creates the main task in **ClickUp**, and applies **conditional subtask logic** — 3 subtasks for Starter, 5 for Growth, or the full task list for Premium — then notifies the #operations Slack channel.

### 3️⃣ Status Sync Automation
Triggers when a ClickUp task is marked "Done." Automatically updates the client's status in Airtable from "Onboarding" to "Active" and notifies the assigned Account Manager on Slack — removing the need for manual status tracking.

### 4️⃣ Weekly Audit (Scheduled)
Runs every Friday at 9:00 AM via a Cron trigger. Scans ClickUp for any onboarding tasks still open, compiles a summary of affected clients and wait times, and sends it directly to the Account Manager on Slack — so no client falls through the cracks.

### Error Handling
All API-dependent steps (Airtable, ClickUp) include failure detection. If a call fails (auth error, rate limit, empty response), the system automatically alerts the internal Slack channel with the affected client, the failing stage, and the error message — instead of failing silently.

---

## Tech Stack

| Category | Tools |
|---|---|
| Automation | n8n |
| Database | Airtable (via HTTP Request API) |
| Task Management | ClickUp |
| Communication | Slack |
| Data Intake | Tally (conditional logic by plan) |

---

## Impact

- Reduced onboarding processing time from **~2 hours to near-zero manual effort** per client.
- Eliminated data-entry errors from manual copy-paste between systems.
- Gave the Account Management team automatic visibility into stalled onboardings via the weekly report, instead of manual weekly reviews.

---

## Contents

- 📊 `/diagrams` — Process map (before automation) and n8n workflow canvases for all 4 automations
- 📄 `1. AR Inicio del onboarding.json` — Welcome & briefing automation
- 📄 `2. AR Registro y organización interna.json` — Operations engine (Airtable + ClickUp + Slack)
- 📄 `3. AR Cierre del onboarding.json` — Status sync automation
- 📄 `4. AR Reporte semanal de seguimiento.json` — Weekly audit (scheduled)
- 🎥 [Demo video](#) — walkthrough of the full system in action *(add your Loom/YouTube link)*

*All credentials, API keys, and real client data have been removed from the exported JSON files.*