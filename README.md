# PDGO-App-26-27

CSULA senior capstone project (2026–2027) for the **LA County Public Defender's Office**: expanding their existing PD GO Power Platform app into a unified, Jira/helpdesk-style ticketing system for Administrative Services — Facilities, Procurement, Employee Safety, Warehouse/Records, and Administrative Support.

See [`requirements.md`](./requirements.md) for the full breakdown of scope, functional requirements, and open questions, distilled from the sponsor's project proposal.

## What's in this repo

- **`PDGo/`** — a Power Apps canvas app inherited from a prior cohort (built for a Santa Barbara PD project, then reused as a starting point here). It's built through the Power Apps Studio drag-and-drop designer; the `Src/*.fx.yaml` files are Studio's exported, source-controllable representation of each screen, not hand-authored app code. It already has working modules for:
  - Facility Requests (submit + admin assign/manage/categorize)
  - Time Off requests
  - Training Tracker (MCLE trainings, check-in/out, certificates)
  - Profile / Team / Supervisor screens
- **`PDGoTicketing/`** — **the current team app** ("Faculty Bucket List" in the CSULA tenant, data in `csula.sharepoint.com/sites/team-12-pd-go`). Unpacked `.msapp`; the readable source is `PDGoTicketing/Src/*.pa.yaml`, one file per screen. The rest (`Controls/`, `References/`) is Studio's internal state — don't hand-edit it.
- **`PDGoTicketing.msapp`** — the same app as a single file, kept as a restorable snapshot (Power Apps Studio → Open → Browse files).
- **`requirements.md`** — project requirements derived from the sponsor's proposal document.

> This repo is **public**. Mock/test data only — never commit the client's real emails, names, or ticket data.

## Workflow for `PDGoTicketing/`

The live app in Power Apps (co-authored in Studio) is the source of truth; git holds snapshots of it so changes are reviewable and diffable.

1. **Branch** off `main` for your work: `git checkout -b <name>/<feature>`.
2. **Snapshot** the app into `PDGoTicketing/` — either export from Studio (File → Save as → download `.msapp`) and unpack it, or pull the live session's YAML with the Canvas Authoring `sync_canvas` tool.
3. **Commit** and open a PR. The diff of `Src/*.pa.yaml` shows exactly which formulas and controls changed.
4. Merging a PR does **not** change the live app by itself. Changes reach the app by being made in Studio (or pushed into a co-authoring session), then snapshotted again.

**Heads up:** the app's data connections currently point at a prior cohort's SharePoint site (`2023-2024SBPDGoDevelopment`), not an LA County PD data source. That needs to be sorted out before the existing screens will actually work for this project — see the open questions at the bottom of `requirements.md`.

## Working with the Power Apps project

`PDGo/` is in Power Apps' source-control (unpacked) format. To open and edit it visually:

1. Install the [Power Platform CLI](https://learn.microsoft.com/power-platform/developer/cli/introduction) (`pac`) if you don't have it.
2. Pack it back into a `.msapp` to open in Studio:
   ```
   pac canvas pack --sources PDGo/Src --msapp PDGo.msapp
   ```
3. Open `PDGo.msapp` in [Power Apps Studio](https://make.powerapps.com), or import it into your own environment.
4. After making changes in Studio, re-export back to source format:
   ```
   pac canvas unpack --msapp PDGo.msapp --sources PDGo/Src
   ```

Commit the unpacked `Src/` files, not the `.msapp` binary, so changes stay diffable in git.

## Git history note

This repo's history was squashed to a single fresh commit at the start of this cohort — the previous team's history was mostly auto-generated "Commit"/"SmartMerge" noise from Power Apps' GitHub integration and wasn't worth carrying forward. `main` requires 2 PR approvals to merge (dismiss-stale-reviews is on), matching how code review worked at Snap.
