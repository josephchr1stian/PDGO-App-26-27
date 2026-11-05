# PDGO-App-26-27

CSULA senior capstone project (2026–2027) for the **LA County Public Defender's Office**: a unified, Jira/helpdesk-style ticketing system for Administrative Services — Facilities, Procurement, Employee Safety, Warehouse/Records, and Administrative Support.

See [`requirements.md`](./requirements.md) for scope, functional requirements, and open questions.

## What's in this repo

- **`PDGoTicketing/`** — the team app ("Faculty Bucket List" in the CSULA tenant, data in `csula.sharepoint.com/sites/team-12-pd-go`), as an unpacked `.msapp`. The readable source is `PDGoTicketing/Src/*.pa.yaml`, one file per screen. The rest (`Controls/`, `References/`) is Studio's internal state — don't hand-edit it.
- **`PDGoTicketing.msapp`** — the same app as a single file, kept as a restorable snapshot (Power Apps Studio → Open → Browse files).
- **`requirements.md`** — project requirements derived from the sponsor's proposal.

> This repo is **public**. Mock/test data only — never commit the client's real emails, names, or ticket data.

## Workflow

The team app in Power Apps (co-authored in Studio) is the source of truth. Git is the layer in between: snapshots of the app, so changes are reviewable and diffable before they reach it.

1. **`main`** = the latest snapshot of the team app.
2. **Branch** for your work: `git checkout -b <name>/<feature>`. Do the work in your own copy of the app (Studio → File → Save as), not the team app.
3. **Snapshot** your copy into `PDGoTicketing/` on your branch — pull the live session's YAML with the Canvas Authoring `sync_canvas` tool, or download the `.msapp` and unpack it.
4. **Open a PR.** The diff of `Src/*.pa.yaml` shows exactly which formulas and controls changed.
5. **Apply** the approved changes to the team app (in Studio, or pushed into its co-authoring session), then snapshot the team app to `main` again.

Merging a PR does **not** change the team app by itself — step 5 does.
