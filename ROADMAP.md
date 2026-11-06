# PD GO Roadmap

What's done, what's in flight, what needs another look, and what's next. Keep it current: when something ships, move it to **Done** with the date; when you find a problem, add it to **Revisit**.

Last updated: 2026-11-06

## Done

- [x] **Git layer for the team app.** `main` holds snapshots of the live Studio session (`PDGoTicketing/Src/*.pa.yaml`), so changes are diffable. See the workflow in the [README](./README.md).
- [x] **New hub** (`Pdgo Hub 10-30-26`) with one request screen per service family: Facilities, Procurement, Employee Safety, Warehouse, Administrative Support. Login goes to the hub.
- [x] **Ticket detail screen** (`TicketDetail_11_05`). My Requests opens it. Editing is gated by `CanEditTicket` (Admin, Supervisor, or the assignee); admins can reassign.
- [x] **Civic Center parking waitlist, core.** Parking is a normal ticket in `AdministrativeServiceRequests` (Facilities → Parking Request → Civic Center).
  - Status is set to *Waiting List*.
  - A row is added to the `Waitlist Queue` list (ticket ID only, no personal info).
  - The ticket detail shows "You're #N on the Civic Center waitlist", counted live from the queue.
  - Duplicate requests are blocked.
- [x] Removed the placeholder `PDGOTicketTypes` list.
- [x] **Waitlist admin actions** on the ticket detail (2026-11-06): *Offer spot* → *Spot accepted*, plus *Remove from waitlist*. Buttons and label read the `Waitlist Queue`, not ticket Status.
- [x] My Requests shows admins/supervisors both their assigned and submitted tickets.
- [x] Removed "Current Tickets" (old `UserHome`) from the menus; one hub for everyone, content by role. Ticket detail Back returns to the hub.
- [x] My Requests table: open tickets first (newest first), closed at the bottom with a ✓ icon; columns Ticket # / Request / Details / Status / Submitted / Last update, plus Assigned to / Requested by for admins.
- [x] Waitlist duplicate check reads `Waitlist Queue` (one active spot per account).
- [x] Removed the hardcoded "Admin Sign In" password button; admin access comes from the `PDGo Users` role.
- [x] `TicketRequestScreen` shows only the signed-in user's tickets.

## In progress

- [ ] **Auto-assign by Facilities subcategory**: a Power Automate flow (teammate). Next step: load-balance between multiple handlers for the same subcategory.
  - The flow must not set Status to *Assigned* on tickets that are *Waiting List* or *Offered*. It can still fill in AssignedTo. Otherwise requesters see "Assigned" while they're still in line.
- [ ] **Privacy**: users can read only their own tickets. Plan: SharePoint item-level permissions on `AdministrativeServiceRequests` ("Read items that were created by the user").
  - `Waitlist Queue` stays readable by all but editable only by its creator.
  - Lock `PDGo Users` / `PDGOCategories` to read-only for regular users.
  - Owners are unaffected. Ticket handlers need a "PD GO Resolvers" group with *Override List Behaviors*.
  - On hold until the team agrees.

## Revisit

- [ ] **RequestedBy vs. test logins.** Request forms save `User().Email`, but My Requests filters on `varEmail` (the test-mode login). With a test login (e.g. joe@gmail.com) your own tickets don't show. Fix: save `varEmail` on all five request screens, plus the waitlist duplicate check.
- [ ] **Waitlist position wording** says "You're #N" even when an admin is viewing.
- [ ] **Old screens to retire** once nothing links to them: `ParkingRequest`, `UserHome`, `MainScreen1`, `TicketDetailScreen`, `Pdgo Hub`, `Login New Hub` (its Navigate formula is broken), the template/test screens, and the two `-----` screens.
- [ ] **Unused lists and data sources**: `Parking Requests`, `Parkest request Test`, `Ticketdisplaytest`, `PDGOTickets`, `Faculty Bucket List`, `Locksmith Requests`. Remove `PDGOTicketTypes` from the app's Data panel; the list is gone.
- [ ] **Test mode** is on (`varTestMode = true` in App.OnStart). Turn off for UAT/handoff.
- [ ] **Priority.** The team proposes dropping it; FR-06/FR-09 mention it. Confirm with the sponsor.
- [ ] **Who approves Civic Center parking**: the screen text says Joe/Deepak, the notes say Vidal.
- [ ] **Modern table limits**: column formulas can't use Coalesce/IsBlank, and rows can't be shaded. A gallery would allow shaded closed rows and status pills.
- [ ] **App checker** cleanup: unused variables, cross-screen references in the template screens.

## Future

- [ ] **Analytics** (FR-09). Resolve rate, time to assign/resolve, open workload, aging/overdue, volume by service and location, reassignment rate. Likely Power BI on the SharePoint lists.
- [ ] **Satisfaction survey.** Email a short survey after a ticket is submitted (or closed). Store responses in a SharePoint list, report them with the analytics above.
- [ ] **Waitlist extras**
  - Offer expiry, e.g. 48 hours to accept before it passes to the next person.
  - A *Release spot* button for permit holders.
  - An email when a spot is offered.
  - Admin queue view in the app.
- [ ] **Notifications** (FR-07). Submission confirmation and status-change emails.
- [ ] **Configuration-driven categories and routing** (FR-10). Admins add categories and handlers without code changes.
- [ ] **Stretch scope**: Box knowledge base, Workvivo space, "Ask PD Admin" assistant. See [requirements.md](./requirements.md) §3.2.
