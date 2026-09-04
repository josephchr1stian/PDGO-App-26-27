# PD GO: Administrative Services Hub — Requirements

**Sponsor:** LA County Public Defender's Office
**Academic partner:** Cal State LA, 2026–2027 Capstone Cohort
**Source document:** `PD GO Admin Services Hub_Updated 81526.docx` (sponsor proposal)

This document translates the sponsor's proposal into working requirements for the student team. It is a starting point, not a locked spec — confirm anything marked **[open question]** with the Project/Business Liaison before building against it.

## 1. Project Summary

The Public Defender's Office wants PD GO — an existing Power Platform app already used for remote-work, overtime, and comp-time requests — expanded into a single, Jira/helpdesk-style ticketing system for Administrative Services. Today, requests for facilities, procurement, safety, warehouse, and admin support come in through email, Teams, phone calls, and paper forms, with no shared intake, tracking, or reporting. The project replaces that with one front door: employees submit a request, get a ticket number, and track it through resolution, while admin staff work it through configurable queues.

Guiding principle from the sponsor: **one front door, one knowledge source, one consistent employee experience.**

## 2. Stakeholders

| Role | Person / Group | Responsibility |
|---|---|---|
| Executive Sponsor | Deepak Budwani, Administrative Deputy II | Vision, scope, executive decisions, final acceptance |
| Project / Business Liaison | Joseph Ferrell, Administrative Services Manager II | Weekly liaison, backlog, requirements, testing, acceptance |
| Business SMEs | Steve Hernandez, Maxwell Owachgui, Robert Dickerson, Vidal Vega, Tom Laguna | Facilities / Safety / Procurement / Warehouse / Admin Support process knowledge, UAT |
| IT / Platform SMEs | LA County PD IT | Power Platform, Box, M365, Teams, identity, security, integration constraints |
| Student Team + Faculty Advisor | CSULA capstone cohort | Discovery, design, development, testing, documentation, demos, handoff |

## 3. Scope

### 3.1 Core scope (primary — this is what the capstone is graded on)

A reusable Administrative Services helpdesk covering five service families:

1. Facilities
2. Procurement
3. Employee Safety
4. Warehouse / Records
5. Administrative Support

The framework must be **configuration-driven** — new categories and routing rules should be addable without redeveloping the app.

### 3.2 Secondary scope (stretch / innovation tracks)

- **Knowledge Foundation:** migrate Administrative Services documents/policies out of scattered SharePoint locations into Box as the authoritative content repository (PD GO stays transactional; it should never become a document store).
- **Workvivo layer:** an Administrative Services space in Workvivo for announcements and navigation into PD GO / Box — communications only, not a second knowledge repository.
- **"Ask PD Admin" AI assistant (optional):** natural-language Q&A grounded in approved Box content, with Box AI / agentic capabilities or M365 Copilot as possible engines. Hard rule from the sponsor: the assistant may retrieve and explain approved knowledge, but must never invent policy or make final safety, procurement, fiscal, personnel, or legal decisions — it routes those into PD GO as a real request.

### 3.3 Out of scope (unless the sponsor says otherwise)

- Anything outside the five listed service families.
- PD GO becoming a general document repository (that's Box's job).
- The AI assistant making binding decisions on its own.

## 4. Functional Requirements

| ID | Requirement | Description |
|---|---|---|
| FR-01 | Request Submission | Authorized staff can submit an Administrative Services request from desktop or mobile. |
| FR-02 | Comprehensive Intake | Intake captures requester identity, location, category/subcategory, description, urgency, safety impact, accessibility impact, and supporting attachments. |
| FR-03 | Conditional Fields | Additional fields/questions appear dynamically based on the selected request category. |
| FR-04 | Unique Request Number | A unique ticket number is generated and returned to the requester on submission. |
| FR-05 | Smart Routing | Rules-based routing sends each request to the correct queue/owner based on category, location, and urgency — no manual triage required for the common case. |
| FR-06 | Operational Management | Admin staff can triage, assign, prioritize, comment on, update, escalate, and close requests, with notes and a due date. |
| FR-07 | Notifications | Requesters get a submission confirmation and milestone/status/closure notifications; no need to check back manually. |
| FR-08 | Common Lifecycle & Queues | All request types share one lifecycle model: **New → Assigned → In Progress → Waiting → Escalated → Resolved/Closed**, with room for service-specific statuses or waiting reasons. |
| FR-09 | Reporting & Analytics | Managers can filter/report by location, category, status, aging, priority, and assignee. Dashboard should surface: open workload, volume by service/location, time-to-triage/assign/resolve, aging/overdue work, manual rerouting rate, vendor/external dependencies, repeat issues, and trends. |
| FR-10 | Configuration Management | Locations, categories, routing rules, and statuses are maintainable by an admin without code changes. |
| FR-11 | Audit & Accountability | The system records ownership, timestamps, material status changes, and closure outcomes for every request. |

## 5. Non-Functional Requirements

- **Fast intake:** a routine request should take roughly two minutes to submit with all required information.
- **Extensibility:** adding a new request category should be a configuration change, not a redevelopment.
- **Transparency:** requesters can always see current status without contacting admin staff directly.
- **Data separation:** transactional data (requests/tickets) lives in PD GO; knowledge/documents live in Box; PD GO links out to Box rather than duplicating content.
- **Permission-aware knowledge:** if the AI assistant is built, it must respect existing content permissions and cite its sources.

## 6. Success Measures / Acceptance Criteria

| Outcome | Acceptance Standard |
|---|---|
| Simple access | A pilot employee can find the right service path without knowing which Administrative Services unit owns it. |
| Fast intake | A routine request can be submitted in ~2 minutes with complete information. |
| Routing quality | A high percentage of pilot requests route correctly without manual reassignment. |
| Transparency | Requester can see current status and gets milestone notifications. |
| Operational visibility | Managers can view open workload, aging, category, location, and assignment. |
| Maintainability | New categories can be added primarily through configuration. |
| Knowledge quality (if piloted) | Pilot Box content has an owner, lifecycle status, permissions, and metadata. |
| AI trust (if piloted) | AI responses are grounded in approved content, respect permissions, cite sources, and escalate when uncertain. |

## 7. Proposed Timeline (from sponsor proposal)

| Phase | Window | Milestone |
|---|---|---|
| 1. Discovery & current-state mapping | Sep – Oct | Current state understood |
| 2. UX + solution architecture | Oct – Nov | Solution blueprint approved |
| 3. Initial prototype / design review | Nov – Dec | Prototype reviewed |
| 4. MVP build + service queues | Jan – Mar | MVP built and tested |
| 5. Knowledge / innovation track | Feb – Apr | Innovation validated |
| 6. UAT + documentation | Apr – May (early) | UAT complete, docs finalized |
| 7. Final handoff + executive demo | Apr – May 1 | Capstone presentation, May 1 |

## 8. Current State of the Inherited App — Gaps & Open Questions

The repo includes a Power Apps canvas app (`PDGo/`) inherited from a prior cohort. It already has working modules for Facility Requests (user submission + admin assign/manage/categorize), Time Off requests, a Training Tracker (MCLE trainings, check-in/out, certificates), and Profile/Team screens. It was built primarily through the Power Apps Studio drag-and-drop designer — the `.fx.yaml` files under `PDGo/Src/` are Studio's serialized/diffable export of that design, not hand-written app code.

Known gaps to resolve early, before building on top of it:

- **[open question]** The app's data connections point at `https://csula.sharepoint.com/sites/2023-2024SBPDGoDevelopment` — a prior Santa Barbara PD cohort's SharePoint site. This needs to be re-pointed to a real LA County PD data source (new SharePoint site, Dataverse, or otherwise) before any of the existing screens will work for this project.
- **[open question]** The Facility Request module is the closest existing analog to the unified ticketing system, but it only covers one of the five target service families (Facilities). Procurement, Employee Safety, Warehouse/Records, and Administrative Support have no existing screens.
- **[open question]** There's no rules-based routing engine, unified lifecycle across services, or management dashboard yet — these are core, ungraded-if-missing pieces of the core scope (Section 3.1) and don't exist in the inherited app.
- **[open question]** Whether the team keeps building in Power Apps canvas (matching the sponsor's stated core technology) or considers a different architecture is a decision to confirm with the Project/Business Liaison, not something to assume.
