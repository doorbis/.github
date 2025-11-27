# Doorbis ClosePilot™

**Doorbis ClosePilot™** is an AI-powered real estate transaction coordination platform for brokers, agents, teams, builders, and transaction coordinators. It turns messy timelines, documents, and multi-channel communications into a single, coordinated “auto-TC” that keeps every deal on track.

🌐 Live product & demo: **[https://doorbis.com](https://doorbis.com)**

---

## What ClosePilot Does

### 🧭 Unified Transaction Workspace

Each transaction has a single workspace with:

* Timeline view: milestones, contingencies, and key dates with stage-aware planning (lead → under contract → closing).
* Role-based task views: tasks grouped by buyer, seller, lender, escrow, builder, and TC roles. 
* Calls & notes: lightweight call logging per transaction, including direction, outcome, timestamps, and notes. 
* E-sign packets, documents, and “Docs & Compliance” views to keep paperwork and e-sign envelopes connected to the timeline. 
* Builder-specific sections for new construction: options, change orders, walkthroughs, orientations, and key handoff milestones. 

The workspace is implemented as a Streamlit page backed by FastAPI services and a multi-tenant PostgreSQL schema with strict row-level security.

---

### 🤖 AI Briefings, Risk & Next Steps

ClosePilot uses OpenAI to act as an automated TC “brain”:

* Daily / on-demand **AI briefings** summarizing escrow health, risk factors, and recommended next steps.
* Stage-aware **task & milestone planners** that generate structured checklists per transaction stage. 
* “Next steps” suggestions that propose new milestones, tasks, and outreach drafts based on current context (timeline, tasks, and recent comms). 

These workflows are wired through a shared OpenAI client (`OpenAIWorkflowClient`) with strict JSON tool schemas for predictable outputs.

---

### 📄 AI Document Intake & Contract Understanding

Upload a signed purchase agreement or new-build contract and ClosePilot will:

* Extract property details, transaction metadata, parties, and key dates. 
* Generate a **proposed timeline** (timeline_milestones) directly from the contract text.
* Map extracted data into a structured payload for property, transaction, builder, community, parties, and timeline, ready to be saved. 

This flows through the **AI Document Intake** page and FastAPI `/docs/upload` + AI orchestration endpoints.

---

### 💬 Multi-Channel Messaging & Party Preferences

ClosePilot centralizes communication across:

* **SMS, Email, WhatsApp, LINE, Messenger, Telegram, Voice, and Web/Party-Portal** channels (configured via a shared `channel` enum and messaging APIs).
* Per-party **communication preferences**: enabled channels, quiet hours, language hints, and consent flags. 
* A Party Directory UI showing contact info, enabled channels, and recent calls per contact. 
* ClosePilot messaging pages for thread-level views and sending transactional updates from the browser. 

Admin users can manage welcome messages, appointment proposals, and review-request templates from a dedicated ClosePilot Admin console. 

---

### 🧾 Intro Packets, Earnest Money, Weekly Status

Core TC workflows are first-class:

* **All-party intro packets** that email key dates, contract links, and role-appropriate intros to agents, clients, lender, title, escrow, and builder teams. 
* **Earnest money tracking**: amount, due date, status, escrow holder, references, and notes, with UI and API for safe updates. 
* **Weekly status digests** that summarize completed milestones and upcoming deadlines and send role-specific updates to selected parties. 

---

### 👥 Party-Facing Portals & Client Reviews

* Secure **Party Portals** per party/transaction with a timeline and messaging view, delivered via signed short links.
* **Client review profiles** and post-close review requests (Google, Zillow, Yelp, etc.) configurable from the Admin UI. 

---

## Architecture Overview

Doorbis ClosePilot is built as a dual-stack app:

* **FastAPI backend** (`api/`):

  * Multi-tenant PostgreSQL with `app.current_tenant` GUC + RLS. 
  * Rich TC schema: tenants, parties, transactions, tasks, communication packets, earnest money, doc checklists, calls, and more. 
* **Streamlit frontend** (`pages/`):

  * Dashboard, transaction workspace, messaging, AI briefing, AI coaching, AI document intake, admin, analytics, party directory, and more.

Supporting utilities handle environment configuration, API base discovery (local vs. cloud), and browser-aware time zone conversion. 

---

## Getting Started (Developers)

> **This org profile README is high-level.** See the main ClosePilot application repository for full setup instructions.

Typical local workflow:

```bash
# API
DOORBIS_API_BASE=http://localhost:8000 \
API_SESSION_SECRET=dev-only-secret \
uvicorn api.main:app --reload

# Streamlit UI
DOORBIS_API_BASE=http://localhost:8000 \
streamlit run doorbis_home.py
```

Required environment values (representative):

* `DOORBIS_DB_DSN` – PostgreSQL connection.
* `API_SESSION_SECRET` / `SESSION_JWT_SIGNING_KEY` – session cryptography.
* `DOORBIS_API_BASE` – FastAPI base URL.

---

## Roadmap (Selected)

* Deeper **DocuSign / e-sign** wiring into timeline & closing package assembly.
* Expanded **builder playbooks** and new-construction workflows (options, delays, warranty handoff).
* Richer **AI coaching & insights** for TCs and team leads (per-tenant analytics + recommendations).
* Further integrations (Calendars, CRMs, phone systems, MLS/IDX).

---

## Company & Licensing

Doorbis.com is a service of **RealAgentic AI LLC**, a California LLC, and part of the **Kossel Corp.** family (Delaware corporation founded in 2022).
Real Advantage Corporation — California DRE Licensed Real Estate Corporation • License ID 02054916.
