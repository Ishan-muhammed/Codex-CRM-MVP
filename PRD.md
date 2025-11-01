# AI‑Powered CRM — MVP PRD (for Agent Handoff)

**Version:** 1.0
**Date:** 30 Oct 2025 (IST)
**Owner:** Product (You)
**Audience:** Engineering, AI/ML, Design, QA, RevOps

---

## 0) One‑Page Summary

Build an **AI‑powered CRM MVP** with a persistent **Assistant Sidebar** that lets sales teams create and update **Leads, Contacts, Accounts, Deals** and **AI Quotations** via tiny guided prompts or natural language. All entities are relational. A **Sales Intelligence Dashboard** shows KPIs (lead funnel → revenue, SLA, productivity). The MVP emphasizes low‑friction data entry, smart lead conversion rules by source (Phone, WhatsApp, Email), and fast insights.

**North‑star outcome (MVP):** reduce manual CRM data entry by **>70%**, first‑response SLA within **2 hours** for **>90%** of new leads, and time‑to‑quote under **5 minutes**.

---

## 0.1) Company Context (Emergence Technology Solutions — UAE)

**Industry focus:** Data network infrastructure & ELV/ICT: AV solutions, structured cabling, fiber, wireless, DR, door access, intruder/security. CRM must support **project‑based B2B sales** with RFP/BOQ‑driven proposals, long cycles, and multi‑stakeholder approvals.

**Key verticals:** Government, Education, Ports/Logistics, Banking/FS (per case studies).

**Regional specifics:** Default currency **AED**, apply **5% VAT** on quotations (configurable), store **TRN** (tax registration number) per tenant, and support **milestone billing** and **AMC** renewals.

**Sales process shape (high‑level):** Inbound **email/phone/WhatsApp** → Discovery → **Site Survey** → Solution Design → Proposal/Compliance → Tech Clarifications → Negotiation → **PO Issued** → Implementation → Handover → **AMC** *(future: handled in Project module; excluded from MVP—after client approval, the record will route to the Project Manager).*

---

## 1) Goals & Non‑Goals

### Goals

* Let users **create/update CRM records** (Lead, Contact, Account, Deal, Quotation) **directly from the Assistant Sidebar** using tiny guided prompts.
* Enforce **source‑aware conversion** rules:

  * **Phone**: auto‑qualify → ask “Convert to Contact?”
  * **WhatsApp**: create as Lead → qualify via phone → ask conversion
  * **Email**: auto‑create **Contact + Account** alongside Lead
* Provide an **AI Quotation Generator** (MVP) linked to Deals.
* Ship a **Sales Intelligence Dashboard** with actionable KPIs.
* Deliver **auditable**, **role‑based**, **multi‑tenant** core with clean APIs.

### Non‑Goals (MVP)

* Advanced CPQ (complex pricing engines), custom product catalogs with tiered approvals.
* Omni‑channel inbox UI (basic connectors/workers only).
* Territory management, forecasting, commissions.
* Mobile apps (responsive web only).

---

## 2) Personas & Primary Use Cases

**Sales Rep (primary):** Create/convert leads, update deals, log activities—all from the sidebar; quotations are created in the **Quote** module via its own chat bar (Lovable‑style).
**Presales Solutions Architect:** Parse RFPs/BOQs, design solution, generate proposal & compliance matrix.
**Sales Manager:** Monitor KPIs, SLA, pipeline health, coach team.
**RevOps/Admin:** Configure fields, automations, permissions, data quality rules.

**Top use cases (MVP):**

1. Create a Lead via sidebar tiny prompt → (Phone/WhatsApp/Email) → follow conversion flow.
2. Convert a Lead → Contact + Account (guided choices).
3. Create a Deal from Contact/Account, select **service lines**, set stage, amount, expected close.
4. Generate an **AI quotation** from a Deal (BOQ/RFP text or file) → PDF output saved to the Deal, with **VAT 5%** applied.
5. View dashboard KPIs with filters (date, owner, source, **service line**, vertical).
6. Auto‑create **Proposal Checklist** & **Site Survey** tasks when relevant.

---

## 3) Scope (MVP Feature Set)

**Channel scope (MVP):** Inbound sources are **Email, Phone, WhatsApp only**. The **website lead form is intentionally excluded** from MVP ingestion; add later in Phase 2 if needed.

### 3.1 Assistant Sidebar (always visible, right side)

**UI:** Buttons: **Leads, Contacts, Accounts, Deals, Dashboard, Projects**.
**Interaction modes:**

* **Click-to-form**: tiny guided prompt (slot-filling) opens as a card.
* **Free text**: “Create a lead: Jane, +971… via email; RFP attached for Meraki Wi‑Fi across campus.”

**Core capabilities:**

* Entity **create/update** with validation and confirmation.
* **Source-aware conversion** prompts post-creation (see §4).
* **RFP detection only (no BOQ parsing here):** flag potential RFP emails and hand off to the **Quote** module’s chat bar; no item extraction in the sidebar.
* **Summaries**: AI summarizes inbound messages/emails into lead notes.
* **Disambiguation**: if duplicates detected or multiple matches, assistant asks clarifying Qs.
* **Audit trail**: every AI action logs a structured event and the final payload.

### 3.2 Core Modules

* **Leads** → may convert to **Contacts** and **Accounts**.
* **Contacts** (people) ↔ **Accounts** (organizations).
* **Deals** (linked to Account and optionally Contact, has stage/amount).
* **Activities/Tasks** (calls, meetings, follow‑ups, to‑dos).
* **Quotations** (files + structured summary), linked to Deals.
* **Projects** *(lightweight MVP)*: container for implementation/handover/AMC info tied to **Deal** (won).

### 3.3 Emergence Service Lines (taxonomy used across Lead/Deal/Quote)

* **IT Strategies**
* **Data Centre Migration**
* **Structured Cabling**
* **Fiber Solutions**
* **DR Solutions**
* **CCTV Solutions**
* **Intruder Systems**
* **Door Access Systems**
* **Wireless Solutions**
* **Firewall & Monitoring**
* **AV Solutions**

### 3.4 Sales Intelligence Dashboard

* **Performance:** total leads (D/W/M), conversion (Lead→Deal→Client), revenue per rep, revenue by source & **service line**.
* **Client insights:** new clients this month, high‑value clients, LTV (heuristic MVP).
* **SLA:** first‑response time, leads contacted within 2h, missed SLA, **proposal turnaround time (target ≤ 72h)**.
* **Ops:** tasks created vs completed, pending actions, workload, avg lead cycle time, **site surveys scheduled vs completed**.
* **Quotation funnel:** **expected quotations**, quotations drafted/sent, **client replies after quote**, reply rate, acceptance rate, average reply time.

### 3.5 Automations (MVP)

* Skip auto‑acknowledge for new leads; Email ingestion handled via n8n agent (no auto‑reply) and WhatsApp auto‑acknowledge disabled.
* Auto task: **“Call/qualify lead within 2 hours.”**
* **If lead source = email with attachments:** parse RFP/BOQ and create a **Proposal Checklist** task list (deadline = RFP due‑date).
* **Schedule Site Survey** task when service lines include cabling/wireless/access control.
* Auto follow‑ups for uncontacted leads: **D+1, D+3, D+7** (queue tasks).
* Auto status → **Cold** after **14 days** no response.
* Email leads: auto‑create **Contact + Account**.

### 3.6 Relational Modules (NEW)

Add four relational, linkable modules. **Every item can link to one or more records** (Lead, Contact, Account, Deal) and **appears in each record’s timeline**.

* **Meetings/Calendar:** schedule/reschedule/cancel; log outcomes & attendees; **Calendar page (Month/Week/Day)** inside CRM; drag–drop to reschedule; create **Client Meeting** or **Site Visit** entries; two‑way sync with Google/Outlook; show on record timeline.
* **Tasks:** assign to users/roles; due dates; priorities; reminders; SLA flags; bulk complete/reschedule.
* **Notes:** rich‑text with **@mentions**; paste images; file attachments; edit history.
* **Files/Docs:** versioned storage for quotes, invoices, site photos, contracts; folder tags and categories.

**Stage automation:** On the **first** Client Meeting/Site Visit created for a Deal, auto‑advance that Deal to the **Consultant** stage (second stage). If the Deal is already at or beyond the second stage, do nothing. (Configurable per tenant: `second_stage_key` defaults to `consultant`.)

### 3.7 WAU — Work Assignment Utility (Smart Auto‑Assignment) (NEW)

**Goal:** Auto‑assign the right owner and create follow‑up tasks with SLA, fairly distributing workload while respecting skills/territory and pipeline context.

**When it triggers:**

* On **Lead creation** (assistant or API)
* On **Deal creation** (optional, per tenant)
* On **Quote sent** (optional task: “Follow up in 2 days”)
* On **Owner change** (reassign all open tasks to new owner)

**User experience:**

1. Assistant asks: **“Who should I assign this to?”** (picker with search + load hints).
2. If skipped, WAU auto‑selects an assignee and creates default tasks (e.g., **Call within 2 hours**), adding SLA flags and reminders.

**Assignment algorithm (hybrid):**

* **Priority rules engine** (first match wins) → else **Weighted Round‑Robin** with a scoring formula.

**A) Rules engine (admin‑configurable, ordered)**

```json
{
  "rules": [
    {
      "id": "serviceline_wireless_region_uae",
      "if": { "service_line": ["Wireless Solutions"], "region": ["UAE"] },
      "then": { "assign_to": ["user_ali", "user_samira"], "strategy": "least_utilization" }
    },
    { "id": "existing_pipeline_owner", "if": { "has_open_deals_for_account": true }, "then": { "assign_to": "current_pipeline_owner" } }
  ],
  "fallback": { "strategy": "weighted_rr" }
}
```

**B) Weighted Round‑Robin (fallback) — scoring**
For each candidate user `u` in the tenant/team pool:

```
score(u) =  w1*(1 - utilization_u)
          + w2*skill_match_u
          + w3*territory_match_u
          + w4*pipeline_owner_match_u
          + w5*recent_sla_performance_u
          + w6*availability_u
```

* `utilization_u` = open_tasks / capacity (capped at 1.5)
* `skill_match_u` ∈ {1, 0.5, 0}
* `territory_match_u` ∈ {1, 0}
* `pipeline_owner_match_u` ∈ {1, 0}
* `recent_sla_performance_u` ∈ [0,1] (e.g., last 30 days on‑time %)
* `availability_u` ∈ {1 if not OOO and calendar free at next 2h slot, else 0}

**Default weights (editable):** `{w1:0.35, w2:0.2, w3:0.15, w4:0.15, w5:0.1, w6:0.05}`

**Task payload annotations:**

* `task.auto_assigned = true`
* `task.assigned_by_rule_id = <rule or "weighted_rr">`
* `task.assignment_reason = "least_utilization|territory_match|pipeline_owner"`

**Out‑of‑office & capacity handling:**

* Users with `status=ooo` or `capacity=0` are excluded.
* If all candidates exceed capacity, **escalate to manager** and assign temporarily to the least utilized.

**Admin UI:**

* Visual rule order with drag‑drop; JSON editor; test‑run panel (simulate a lead).
* Capacity per user; skills & territories; default task templates by source/service line.

**Default task templates (MVP):**

* On Lead (phone/whatsapp/email): **Call within 2 hours** (reminder at 90m; SLA flag).
* On Quote sent: **Follow up in 2 days**.
* On Site Visit scheduled: **Upload survey notes/photos** by EOD.

**Default MVP Config — 5‑Rep Team (Simple & Fair)**

* **Eligible pool:** all 5 sellers with role = `rep`.
* **Strategy:** pure **round‑robin** turn order `[rep1, rep2, rep3, rep4, rep5]`.
* **Skip logic:** if assignee is **OOO** or `open_tasks ≥ capacity`, skip to next.
* **Instant action:** auto‑create **“Call within 2 hours”** with a **90‑minute reminder** and `sla_flag=true`.
* **Owner changes:** reassign all **open** tasks to the new owner.
* **Metrics target:** fairness ≈ **20%±5%** of new leads per rep over any 2‑week window.

**Example tenant config (fill with real user IDs):**

```json
{
  "wau_mode": "round_robin_simple",
  "team_pool": ["rep_A","rep_B","rep_C","rep_D","rep_E"],
  "skip_if": { "ooo": true, "at_or_above_capacity": true },
  "defaults": { "create_task_call_2h": true, "reminder_minutes": 90, "sla_flag": true },
  "reassign_on_owner_change": true,
  "fairness_target": { "per_rep_pct": 20, "tolerance_pct": 5, "window_days": 14 }
}
```

### 3.8 Client Overview (NEW)

Openable from **Lead, Contact, Account, or Deal**. A single hub showing everything tied to the client:

* **Summary:** owner, lifecycle stage, last touch, next action.
* **Docs:** quotes, proposals, contracts.
* **Invoices:** status (draft/sent/paid/overdue), amounts.
* **Images:** site photos (album view).
* **Meetings & Tasks:** upcoming + overdue; quick actions (**Complete**/**Reschedule**).
* **Activity timeline:** emails/WhatsApp, calls, notes, file uploads, AI summaries.

**Rules:**

* If opened from a **Lead/Deal**, also show the **parent Account** panel.
* **“Add to Overview”** action on every file/note allows pinning key items.
* **Invoices and Quotes** auto‑link to the correct **Account** and **Deal** and appear here.

### 3.9 Calendar (NEW)

A dedicated calendar inside CRM with **Month/Week/Day** views. Users can create **Client Meetings** and **Site Visits** directly from the calendar or from any record. Events appear on the relevant records’ timelines and sync two‑ways with Google/Outlook.

**Key capabilities:**

* New event drawer with fields: title, **kind (Client Meeting | Site Visit)**, date/time, attendees, location, related Account/Contact/Deal, notes.
* Drag–drop to reschedule; conflict warnings; one‑click "Join Call" for online meetings.
* First creation of a Meeting/Site Visit linked to a Deal triggers **stage auto‑advance to the Consultant (second) stage** (idempotent).

**Out of scope (MVP):** room resources, advanced recurrence exceptions.

## 4) Lead Source → Conversion Logic

| Source   | At creation                             | Post-creation assistant action                                                                              |
| -------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Phone    | Mark **qualified=true**                 | **If created via Assistant:** Ask “**Create Contact now?**” [Yes] [No] → Yes creates **Contact** and links. |
| WhatsApp | Create lead (qualified later via phone) | **After phone qualify (if via Assistant):** Ask “**Create Contact now?**” [Yes] [No].                       |
| Email    | Create Lead + **Contact + Account**     | No prompt; show confirmation and links to created records.                                                  |

Duplicate handling: phone/email keys → if existing Contact/Account, propose linking instead of creating new. phone/email keys → if existing Contact/Account, propose linking instead of creating new.

---

## 5) Data Model (Relational)

**Global:** `tenant_id`, `id` (ULID), `created_at`, `updated_at`, `owner_id`, `source`, `tags[]` on major entities.

### 5.1 Entities (JSON Schemas)

**Lead**

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "owner_id": "ulid",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "phone": "string",
  "company": "string",
  "source": "phone|whatsapp|email|referral|other",
  "status": "new|contacted|qualified|unqualified|cold",
  "qualified": "boolean",
  "notes": "string",
  "linked_contact_id": "ulid|null",
  "linked_account_id": "ulid|null",
  "region": "string|null",
  "utm": {"campaign": "string|null", "medium": "string|null", "source": "string|null"},
  "custom": {"type": "object"}
}
```

**Contact**

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "phone": "string",
  "job_title": "string|null",
  "account_id": "ulid|null",
  "lifecycle_stage": "lead|mql|sql|client",
  "address": {"city": "string|null", "country": "string|null"},
  "custom": {}
}
```

**Account**

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "name": "string",
  "domain": "string|null",
  "industry": "string|null",
  "phone": "string|null",
  "billing_address": {"city": "string|null", "country": "string|null"},
  "custom": {}
}
```

**Deal**

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "name": "string",
  "account_id": "ulid",
  "contact_id": "ulid|null",
  "stage": "discovery|site_survey|solution_design|proposal|tech_clarifications|negotiation|po_issued|implementation|handover|amc|won|lost",
  "amount": "number",
  "currency": "INR|USD|AED|EUR",
  "expected_close": "date|null",
  "probability": "number|null",
  "custom": {}
}
```

**Quotation** (linked to Deal)

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "deal_id": "ulid",
  "title": "string",
  "items": [
    {"sku": "string|null", "brand": "string|null", "desc": "string", "qty": "number", "unit_price": "number", "tax_pct": "number|null", "discount": "number|null"}
  ],
  "subtotal": "number",
  "tax_total": "number",
  "discount_total": "number",
  "grand_total": "number",
  "currency": "AED",
  "vat_pct": "number",
  "file_url": "string",
  "status": "draft|sent|accepted|rejected",
  "notes": "string|null",
  "terms": {"delivery_weeks": "number|null", "warranty_months": "number|null", "payment_milestones": ["string"]}
}
```

**Activity/Task**

````json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "type": "call|meeting|email|todo",
  "subject": "string",
  "due_at": "datetime|null",
  "owner_id": "ulid",
  "related_entity": {"type": "lead|contact|account|deal", "id": "ulid"},
  "priority": "low|medium|high",
  "sla_flag": "boolean",
  "status": "open|done|skipped",
  "auto_assigned": "boolean",
  "assigned_by_rule_id": "string|null",
  "assignment_reason": "string|null"
}
```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "type": "call|meeting|email|todo",
  "subject": "string",
  "due_at": "datetime|null",
  "owner_id": "ulid",
  "related_entity": {"type": "lead|contact|account|deal", "id": "ulid"},
  "priority": "low|medium|high",
  "sla_flag": "boolean",
  "status": "open|done|skipped"
}
````

**Meeting** (two‑way calendar sync)

````json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "subject": "string",
  "kind": "client_meeting|site_visit",
  "start_at": "datetime",
  "end_at": "datetime",
  "attendees": [{"name": "string", "email": "string"}],
  "location": "string|null",
  "address": {"line1": "string|null", "city": "string|null", "country": "string|null"},
  "outcome": "string|null",
  "related_records": [{"type": "lead|contact|account|deal", "id": "ulid"}],
  "external_event_id": "string|null",
  "status": "scheduled|rescheduled|cancelled|completed"
}
```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "subject": "string",
  "start_at": "datetime",
  "end_at": "datetime",
  "attendees": [{"name": "string", "email": "string"}],
  "location": "string|null",
  "outcome": "string|null",
  "related_records": [{"type": "lead|contact|account|deal", "id": "ulid"}],
  "external_event_id": "string|null",
  "status": "scheduled|rescheduled|cancelled|completed"
}
````

**Note**

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "body": "string",
  "mentions": ["ulid"],
  "files": ["ulid"],
  "related_records": [{"type": "lead|contact|account|deal", "id": "ulid"}],
  "pinned": "boolean",
  "visibility": "team|private"
}
```

**FileDoc** (versioned file)

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "name": "string",
  "url": "string",
  "category": "Quote|Invoice|Contract|Site Photo|Other",
  "versions": [{"n": "number", "url": "string", "uploaded_at": "datetime", "uploaded_by": "ulid"}],
  "related_records": [{"type": "lead|contact|account|deal", "id": "ulid"}],
  "visibility": "team|private",
  "tags": ["string"]
}
```

**Invoice**

```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "deal_id": "ulid",
  "account_id": "ulid",
  "number": "string",
  "issue_date": "date",
  "due_date": "date",
  "currency": "AED",
  "vat_pct": 5,
  "subtotal": "number",
  "tax_total": "number",
  "grand_total": "number",
  "status": "draft|sent|paid|overdue",
  "file_url": "string"
}
```

**User**

````json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "name": "string",
  "email": "string",
  "role": "admin|manager|rep",
  "capacity": 40,
  "skills": ["Wireless Solutions", "Structured Cabling"],
  "territories": ["UAE", "KSA"],
  "service_lines": ["Wireless Solutions", "AV Solutions"],
  "status": "active|ooo|inactive",
  "manager_id": "ulid|null"
}
```json
{
  "id": "ulid",
  "tenant_id": "ulid",
  "name": "string",
  "email": "string",
  "role": "admin|manager|rep"
}
````

**Relationship highlights**

* Lead (0..1) → Contact; Lead (0..1) → Account
* Contact (M) ↔ Account (1)
* Deal (M) → Account (1), (0..1) → Contact
* Quotation (M) → Deal (1)
* Meeting/Task/Note/FileDoc (M) → **[Lead|Contact|Account|Deal]** (one or many)
* Invoice (M) → Deal (1), Account (1)

## 6) Assistant: Intents, Slots, Tools Assistant: Intents, Slots, Tools

### 6.1 Intent Taxonomy (MVP)

* `create_lead`, `update_lead`, `convert_lead`
* `create_contact`, `create_account`
* `create_deal`, `update_deal`, `move_deal_stage`
* `create_task`, `list_tasks_due`, `reassign_owner`
* `schedule_meeting`, `update_meeting`
* `create_note`, `attach_files`, `upload_site_photos`
* `show_client_overview`, `pin_to_overview`
* `show_dashboard`

### 6.2 Slot Schema (examples)

* **Lead.create** → `first_name*`, `last_name`, `email`|`phone`*, `company`, `source*`, `notes`
* **Deal.create** → `name*`, `account_id*`, `contact_id`, `amount*`, `currency*`, `stage*`, `expected_close`
* **Meeting.schedule** → `subject*`, `start_at*`, `end_at*`, `attendees[]`, `related_records[]`
* **File.upload_site_photos** → `account_id*`, `related_records[]`, `files[]`, `notes`, `visibility` (examples)
* **Lead.create** → `first_name*`, `last_name`, `email`|`phone`*, `company`, `source*`, `notes`
* **Deal.create** → `name*`, `account_id*`, `contact_id`, `amount*`, `currency*`, `stage*`, `expected_close`
* **Quotation.generate** → `deal_id*`, `items[]` or `boq_text`, `discount`, `tax_pct`, `terms`

`*` = required. Assistant asks follow‑ups for missing required slots.

### 6.3 Tool (Function) Interface (for LLM function‑calling)

```json
{
  "name": "create_lead",
  "description": "Create a lead and return the record.",
  "parameters": {
    "type": "object",
    "properties": {
      "first_name": {"type": "string"},
      "last_name": {"type": "string"},
      "email": {"type": "string"},
      "phone": {"type": "string"},
      "company": {"type": "string"},
      "source": {"type": "string", "enum": ["phone","whatsapp","email","referral","other"]},
      "notes": {"type": "string"}
    },
    "required": ["source"]
  }
}
```

*Similar tool specs for:* `convert_lead`, `create_contact`, `create_account`, `create_deal`, `update_deal`, `move_deal_stage`, `generate_quotation`, `create_task`.

### 6.4 System Prompt (starter)

> You are the CRM Assistant. Always: (1) infer intent from the clicked button or user message, (2) collect missing required fields one at a time, (3) validate emails/phones superficially, (4) call the appropriate tool, (5) confirm the result with links and the next best action, (6) write a brief activity note.
> **WAU:** On `lead.created`, ask “Who should I assign this to?”. If skipped, auto-assign by rules (owner load, role/territory, round-robin, pipeline owner) and create default tasks (Call in 2h) with reminders and SLA flags. If ownership changes, reassign all open tasks to the new owner.
> **Source rules:** If `source=email`, after `create_lead` also call `create_contact` and `create_account` with mapped fields (no extra prompt). If `source=phone` or `source=whatsapp` **and the lead was created via Assistant**, immediately prompt: “Create Contact now? [Yes][No]”. If Yes, call `create_contact` and link it to the Lead. Account creation can happen later from Contact/Deal flows. For duplicates (same email/phone/domain), offer to link instead of creating.
> **Site visit uploads:** When the user says they visited a client site and uploads images, store them as `FileDoc` under the Account, link to related Lead/Deal (if any), tag `category=Site Photo`, add a Note summarizing the visit with thumbnails, and surface them in Client Overview.** When the user says they visited a client site and uploads images, store them as `FileDoc` under the Account, link to related Lead/Deal (if any), tag `category=Site Photo`, add a Note summarizing the visit with thumbnails, and surface them in Client Overview.

### 6.5 Example Guided Prompt (Lead)

* Card fields: First name*, Last name, Email/Phone*, Company, Source (select), Notes.
* Submit → Assistant: *“Lead created. **Create Contact now?*** → [Yes] [No]

  * **Yes** → create **Contact** using mapped fields; link to Lead.
  * **No** → keep as Lead; offer next best actions (Schedule Meeting, Create Deal).

> Note: **Account** can be created later from Contact/Deal flows. Email-source leads still auto-create **Contact + Account**.

### 6.6 Error & Safety

* If tools fail, show inline error with retry and provide minimal manual form.
* Log full tool request/response (redact PII for logs if required by policy).

---

### 6.6 Site Visit Uploads (Assistant Flow)

1. User: “Visited client site, uploading photos.”
2. Assistant: ask for **client name** → resolve **Account** (and related Lead/Deal).
3. User uploads images.
4. Assistant: create **FileDoc** entries, link to Account (+ Lead/Deal), tag **Site Photo**, add a **Note** with summary + thumbnails, and confirm.

**Metadata example saved per upload**

```json
{
  "record_type": "Account",
  "record_id": "<account_id>",
  "related_records": [{"type":"Lead","id":"<lead_id>"},{"type":"Deal","id":"<deal_id>"}],
  "category": "Site Visit",
  "files": [{"name":"IMG_1234.jpg","url":"<file_url>"}],
  "uploaded_by": "<user_id>",
  "uploaded_at": "<ISO8601>",
  "notes": "Exterior + lobby measurements; power supply panel photo.",
  "visibility": "team"
}
```

## 7) API Surface (MVP, REST examples)

**Auth:** OAuth2 (tenant + user), bearer tokens.
**Versioning:** `/v1`.

* `POST /v1/leads`  → create lead
* `POST /v1/leads/{id}/convert` → `{to: "contact_account"|"contact"|"account"}`
* `POST /v1/contacts`, `POST /v1/accounts`
* `POST /v1/deals` ; `PATCH /v1/deals/{id}` ; `POST /v1/deals/{id}/stage`
* `POST /v1/tasks` ; `PATCH /v1/tasks/{id}` ; `POST /v1/tasks/reassign`
* `POST /v1/meetings` ; `PATCH /v1/meetings/{id}` ; `POST /v1/meetings/sync`
* `GET /v1/calendar/events?view=month|week|day&from=...&to=...&owner_id=...`
* `POST /v1/notes` ; `PATCH /v1/notes/{id}` ; `POST /v1/notes/{id}/pin`
* `POST /v1/files` (upload) ; `POST /v1/files/{id}/link` ; `POST /v1/files/{id}/version`
* `POST /v1/quotations` (generate) ; `POST /v1/quotations/{id}/send`
* `POST /v1/invoices` ; `PATCH /v1/invoices/{id}`
* `GET /v1/client_overview?account_id=...`
* `GET /v1/dashboard?from=...&to=...&owner_id=...`
* `POST /v1/wau/assign` (explicitly trigger WAU for a payload)
* `GET /v1/wau/rules` ; `PUT /v1/wau/rules` (admin configure rules)

**Events (webhooks):** `lead.created`, `lead.converted`, `contact.created`, `account.created`, `deal.created`, `deal.stage_changed`, `task.created`, `task.reassigned`, `meeting.created`, `meeting.first_for_deal`, `file.uploaded`, `note.created`, `quotation.generated`, `invoice.created`, `wau.assignment`, `wau.reassign`, `wau.escalated`, `sla.breached`.

## 8) AI Quotation Generator (MVP) AI Quotation Generator (MVP)

**Input options:**

1. **Structured items** (array of `{brand?, desc, qty, unit_price, tax_pct?, discount?}`)
2. **BOQ text** or **RFP PDF**: assistant extracts items, **brand constraints** (e.g., Cisco/Meraki), delivery terms, warranty, exclusions, and asks user to confirm.
3. **Files** (logo, site photos) → compose a branded PDF (Emergence template).

**Regional logic:** default **VAT 5%** (configurable), currency **AED**, optional **AMC line item** (annual % of hardware total).

**Output:**

* Calculation (subtotal, VAT, discount, grand total)
* **PDF** stored at `file_url` and attached to **Deal**
* Status flow: `draft → sent → accepted/rejected` (manual toggle ok in MVP)

**Validation:** totals must match items; currency inherited from Deal.
**Next actions:** “Send to client?”, “Schedule **site survey**?”, “Create follow‑up task in 2 days?”

---

## 9) Dashboard KPIs (Formulas)

* **Total Leads** (period) = count(`lead.created_at in range`).
* **Lead→Deal Conversion %** = `unique Deals created with source leads / Leads in period`.
* **Win Rate %** = `Deals won / Deals closed`.
* **Revenue by Rep** = `sum(amount for Deals won by owner)`.
* **Revenue by Source** = aggregation on Lead.source for linked Deals won.
* **Revenue by Service Line** = group by service taxonomy (AV/Cabling/Fiber/Wireless/DR/Access/Intruder).
* **New Clients (month)** = Accounts with first Deal won in month.
* **LTV (heuristic)** = `sum(all won deal amounts by account)` (MVP).
* **First Response Time (avg)** = `avg(first_activity_at - lead.created_at)`.
* **% Contacted <2h** = leads with first_activity_at ≤ 2h.
* **Proposal Turnaround (avg)** = `proposal_sent_at - rfp_received_at`.
* **Missed SLA/wk** = count where first_activity_at > 2h **or** proposal turnaround > target.
* **Tasks: On-time completion %** and **Overdue count**.
* **Meetings: Scheduled vs Completed**, **No‑show rate**.
* **Site visits uploaded** (count) and **latest visit date** per Account.
* **WAU distribution** (rule used, current owner load) and **reassignment count**.

Filters: date range, owner, source, region, **service line**, vertical.

## 10) UX Requirements

### 10.1 Assistant Sidebar

* Fixed at right, 360–420px width.
* Top: entity buttons; Middle: conversation stream; Bottom: input box + mic icon (future).
* Tiny guided prompts appear as cards, not full modals.
* Success: show toast + inline summary + “next best action” buttons.

### 10.2 Dashboard

* Cards for top KPIs; tables for pipeline and SLA; filters row at top; export CSV.

### 10.3 Calendar (NEW)

* Dedicated page with **Month/Week/Day** views.
* New‑event drawer: **kind** (Client Meeting | Site Visit), date/time, attendees, location, related records, notes.
* Drag–drop reschedule; conflict warnings; two‑way sync with Google/Outlook.
* Events display on related records’ timelines.
* **Rule:** first Meeting/Site Visit for a Deal auto‑advances to **Consultant** stage (second stage); idempotent and configurable.

### 10.4 Accessibility & i18n

* Keyboard nav for prompts; status text; left‑to‑right MVP; locale‑aware date/number. & i18n
* Keyboard nav for prompts; status text; left‑to‑right MVP; locale‑aware date/number.

### 10.4 Client Overview (NEW)

* Open from Lead/Contact/Account/Deal (context preserved).
* Left: **Summary** card (owner, lifecycle, last touch, next action).
* Center tabs: **Docs**, **Invoices**, **Images**, **Meetings**, **Tasks**, **Timeline**.
* Right: quick actions (Complete/Reschedule/Assign), **Add to Overview** pin on files/notes.
* Thumbnails grid for **Site Photos**; clicking opens album lightbox.

### 10.5 Overview Page — “Crisp‑style” Layout (Design Plan)

**Reference:** Crisp CRM enhancement case study (minimal, unified, decluttered UI).
**Goal:** A single, glanceable **Client Overview** that is clean, fast, and actionable.

**A) Layout grid**

* **Sticky header** (full‑width): Account name + logo/avatar, stage badge (from Deal if context=Deal), owner chip, last touch, next action button group.
* **Metric chips row**: LTV, Open Deals (count + value), Outstanding Invoices, Proposal Status (latest quote), SLA (first‑response), Site Visits.
* **Two‑column grid**

  * **Main (2/3 width)**: **Activity Timeline** (emails/WhatsApp, calls, notes, file uploads, AI summaries) with filters (All/Comm/Tasks/Files) and inline quick actions (Reply, Complete, Pin to Overview).
  * **Side (1/3 width)**: stacked cards — **Key Info**, **People** (contacts at this account), **Meetings** (upcoming/overdue with reschedule), **Tasks** (due/overdue), **Docs** (Quotes/Contracts), **Images** (site album), **Invoices** (status pills).

**B) Component patterns**

* Cards: 12–16px padding, 8–12px radius, soft shadow at rest; hover raises shadow slightly; use neutral background section, white cards (Crisp minimal vibe).
* Status pills: subtle color + icon; progress chips for proposal and implementation phases.
* Empty states: light illustration, 1‑line guidance, primary CTA (e.g., “Upload first site photo”).
* Load states: skeleton rows for timeline, chip placeholders for metrics.

**C) Interactions**

* **Quick Add** (top‑right): Note, Task, Meeting, Upload Files (drag‑drop), Generate Quote, Raise Invoice. Opens tiny prompts (same pattern as Assistant).
* **Pin to Overview** from any item → pins to a **Highlights** area at top of Timeline.
* **Inline edit** on owner, stage, and next action; auto‑save with toast.
* **Drag‑drop** files/images to **Images** or **Docs**; auto‑tag (category) + link to related Deal/Lead.

**D) Data mapping**

* Header: `Account`, optional `Deal` (stage, amount), `owner_id`, `last_activity_at`, `next_task`.
* Chips: aggregate from `Deal`, `Invoice`, `Quotation`, `Activity` tables; SLA from Automations.
* Timeline: union of `Activity/Task/Meeting/Note/FileDoc` with chronological sort and type icon.
* People: `Contact` where `account_id = current`.

**E) Access & visibility**

* Respect `visibility=private` on Notes/Files. Pinned items obey visibility.
* Manager/Admin can see team; Rep only owned/shared.

**F) Responsive**

* ≥1280px: 2/3 + 1/3 columns; 1024–1279px: 7/12 + 5/12; ≤1024px: single column with header → chips → quick actions → timeline → stacked cards.

**G) Keyboard & a11y**

* Landmarks for header/grid; tab order follows action priority; ARIA for pills and progress; focus ring visible; ESC to close quick‑add prompts.

**H) Analytics events**

* `overview.opened`, `overview.quick_add_clicked`, `overview.pin`, `overview.inline_edit`, `overview.file_dropped`, `overview.filter_changed`.

**I) API usage**

* `GET /v1/client_overview?account_id=...` (pre‑aggregated); fallback to separate reads if needed.
* `POST /v1/notes`, `/v1/tasks`, `/v1/meetings`, `/v1/files` for Quick Add; `POST /v1/files/{id}/link` to relate to Deal/Lead.

**J) Visual tone**

* Minimal typography, ample whitespace, subdued dividers, clear hierarchy—**Crisp‑inspired** but branded for Emergence.

## 11) Permissions & Security

* Roles: **admin**, **manager**, **rep**.
* Reps can only see/edit owned records (plus shared).
* Managers see team; Admin sees tenant.
* **File/Note visibility:** `team` (default) or `private` per item; pins respect visibility.
* PII encrypted at rest; audit log on all writes; soft‑delete.

## 12) Automations & SLA Engine (MVP Behavior)

* On `lead.created`: enqueue **acknowledge** (if channel known).
* **WAU prompt**: ask for assignee; if skipped, auto-assign via rules (owner load/role/territory/round-robin/pipeline owner).
* Create default task: “Call lead within 2 hours.” with reminder + SLA flag.
* If no `first_activity` in D+1/D+3/D+7 → create follow-up tasks.
* If no activity for 14 days → set status `cold`.
* On **owner change**: reassign all **open tasks** to the new owner.
* Meetings sync: push/pull events to external calendar; update status on changes.
* **Calendar rule:** On the **first** Meeting or Site Visit linked to a Deal, auto‑advance `Deal.stage` to the **Consultant** stage (second stage). Configurable: `second_stage_key` (default `consultant`). No change if Deal already at/after that stage. Idempotent per Deal.

Configuration defaults stored per tenant; editable by Admin.

## 13) Telemetry & Observability

* Log: intent detected, slots collected, tool called, duration, success/failure.
* Metrics: assistant success rate, avg prompt rounds, tool error rate.
* **WAU metrics:** assignments by rule, fairness index (Gini on distribution), p95 time‑to‑assign, % escalated due to capacity/OOO, reassignment count, on‑time completion % of auto‑assigned tasks.
* Traces per assistant action chain (create lead → auto‑assign → convert → create deal).

---

## 14) Non‑Functional Requirements

* **Performance:** tool call p95 < 1.5s (ex‑LLM latency); LLM round‑trip p95 < 4s.
* **Availability:** 99.5% MVP.
* **Scalability:** 10 tenants, 100 users, 100k records (MVP baseline).
* **Privacy:** GDPR‑friendly data export/delete at tenant level.

---

## 15) Test Plan (Acceptance Examples)

### 15.1 Lead Creation (Phone Source via Assistant)

1. Click **Lead** → fill minimal fields (phone, first name).
2. Assert record created with `qualified=true`.
3. Assistant prompts: **“Create Contact now? [Yes][No]”**. Choose **Yes** → assert **Contact** created and linked.
4. Task created: “Call lead within 2 hours.”

### 15.2 Lead from Email

1. Free‑text: “Add lead: Priya [priya@acme.com](mailto:priya@acme.com) via email.”
2. Assert **Lead + Contact + Account** created; no conversion prompt.
3. Dashboard reflects 1 new lead (email source) within 5 min.

### 15.3 Quotation

1. On a Deal, click **Quotation** → paste BOQ text.
2. Assistant extracts items (≥80% correct), asks to confirm.
3. Generates PDF, attaches to Deal, sets Quotation status `draft`.

### 15.4 Duplicates

1. Create Lead with existing email.
2. Assistant proposes link; selecting link prevents duplicate creation.

### 15.5 WAU Assignment

1. Create Lead without choosing an assignee → WAU auto‑assigns per rules; default task is created.
2. Change Lead owner → verify all **open tasks** reassign to the new owner.

### 15.6 Site Visit Uploads

1. Say: “Visited client site, uploading photos” → upload 2 images.
2. Files are stored under the **Account**, linked to related **Lead/Deal** (if any), tagged `Site Photo`.
3. A **Note** summarizing the visit (with thumbnails) is created.
4. Images appear in **Client Overview → Images** automatically.

### 15.7 Client Overview

1. Open from Lead/Contact/Account/Deal → hub shows **Summary, Docs, Invoices, Images, Meetings, Tasks, Timeline**.
2. Use **Add to Overview** on a Note → it appears pinned in the Overview.
3. Quotes and Invoices auto‑link to the correct Account & Deal and appear in Docs/Invoices.

### 15.8 Calendar & Stage Auto‑Advance

1. Create a **Client Meeting** for a Deal currently at stage 1.
2. Assert event shows on Calendar and Deal timeline.
3. Verify `Deal.stage` auto‑updates to **Consulting/Discovery** (second stage) once (idempotent).
4. Create a **Site Visit** for the same Deal; stage remains unchanged (already advanced).
5. Reschedule the event via drag–drop; external calendar reflects update.

### 15.9 WAU Auto‑Assignment

1. Create a Lead (service line = Wireless, region = UAE).
2. With rules enabled, assert assignment to the configured user group via **least_utilization**.
3. Disable rules; verify fallback to **weighted round‑robin** and log contains weights & chosen candidate.
4. Mark the top candidate as `status=ooo`; assert WAU skips them and either picks next or escalates.
5. Change Lead owner; assert all **open tasks** reassign to new owner.

### 15.10 WAU 5‑Rep Distribution (MVP Default)

1. Configure `wau_mode=round_robin_simple` and pool `[rep_A, rep_B, rep_C, rep_D, rep_E]`.
2. Inject 50 test leads over 2 days.
3. Verify each rep receives **10±2** leads (≈20%±4%) and that OOO periods cause fair skips without breaking order.
4. Check that each assignment created a **“Call within 2 hours”** task with a **90‑minute reminder** and `sla_flag=true`.

## 16) Rollout Rollout

* **Phase 1:** Assistant for create/convert, basic dashboard, quotation draft.
* **Phase 2:** Send quotation, email/WhatsApp connectors, SLA alerts UI.
* **Phase 3:** Deeper deal analytics, product catalog, approvals.

---

## 17) Open Questions & Risks

* Pricing/tax rules per region for quotes—who owns configuration?
* WhatsApp is out of scope for MVP—no integration or sending; do not connect. Email ingestion via n8n only.
* Email ingestion (IMAP vs webhook relays) for lead creation.
* LLM retries/backoff and hallucination guardrails; PII in prompts.

---

## 18) Starter Payloads (Examples)

**POST /v1/leads**

```json
{
  "first_name": "Arun",
  "last_name": "K",
  "email": "arun@example.com",
  "phone": "+919876543210",
  "company": "Acme India",
  "source": "phone",
  "notes": "Asked for 50 units"
}
```

**Tool call: convert lead**

```json
{
  "lead_id": "01HF...",
  "to": "contact_account"
}
```

**Quotation.generate input**

```json
{
  "deal_id": "01HG...",
  "items": [
    {"desc": "Product A", "qty": 10, "unit_price": 1200, "tax_pct": 18}
  ],
  "discount": 0,
  "terms": "Net 15 days"
}
```

**Quotation output (summary)**

```json
{
  "quotation_id": "01Q...",
  "subtotal": 12000,
  "tax_total": 2160,
  "grand_total": 14160,
  "file_url": "https://.../q-01Q.pdf"
}
```

---

## 19) “Next Best Action” Library (Assistant)

* After Lead create (phone): *Ask convert?*, *Create first task?*, *Open record*.
* After Lead create (email): *Open Contact*, *Open Account*, *Create Deal*.
* After Quotation draft: *Send now?*, *Schedule follow‑up task*, *Edit items*.

---

## 20) Definition of Done (MVP)

* Assistant can create/convert core entities and generate a draft quotation PDF.
* **Relational modules live:** Meetings (two‑way sync), Tasks (SLA flags), Notes (@mentions), Files/Docs (versioned, multi‑link).
* **WAU** prompts/auto‑assigns on Lead creation; default tasks created; open tasks reassign on owner change.
* **Client Overview** accessible from Lead/Contact/Account/Deal; images from site visits surface automatically; pinning works.
* Dashboard shows all KPIs with correct formulas and filters (incl. meetings/tasks/site visits/WAU).
* Automations (2h task, D+1/D+3/D+7, 14‑day cold) fire reliably.
* QA passes acceptance tests, logs/audit in place, roles enforced.
