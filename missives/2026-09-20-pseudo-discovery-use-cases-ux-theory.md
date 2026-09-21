# Pseudo-Discovery — Use-Cases & UX Theory

**Date:** 2026-09-20
**Purpose:** Agenda item 4 (inserted mid-meeting — see [2026-09-20-architecture-brainstorm-meeting-notes.md](2026-09-20-architecture-brainstorm-meeting-notes.md)). Turns the exploratory brainstorm into specific use-cases and a first UX theory, using [use-case-tool-mapping-brainstorm.md](../docs/developer/use-case-tool-mapping-brainstorm.md) as a loose architectural guide and [problem-space-brainstorm.md](../docs/developer/problem-space-brainstorm.md) as source material for use-case selection.

Not a brainstorm doc: this is discovery-toward-decision, staged in `missives/` rather than `docs/developer/`, ahead of a possible later promotion into `manifest/`.

## Scope of this pass

This pseudo-discovery targets a **hypothetical MVP scope** — intentionally larger than the exploratory initial implementation. The exploratory implementation will be a narrower slice, limited to exercising tool connections/interplay and proving the platform capabilities identified in the tool mapping actually work as expected. This document's scope is not that narrower slice; it's the fuller MVP shape the exploratory implementation is a first step toward.

MVP scope, as given:

- Document publishing: committee charter, vision, roadmap.
- Basic info publishing: committee officers, social media/web links, contact info.
- Conduct a meeting: agenda creation, attendance, note-taking, briefing document links, decision records.
- Minutes archive.
- Committee participation interest form.
- CRM with contact types: formal members, occasional participants.
- Draft and post social content: upcoming meeting time/place, drip posts for agenda items, Facebook event creation for a meeting, posting minutes to Facebook.
- Email agenda in advance of a meeting; drip reminders; "interested"/"confirm attendance" email links.
- Shared workspace for committee members: brainstorming/research documents, uploads, links with metadata, quotes with citations.
- Committee newsletter subscription sign-up.

## Specific use-cases

Organized by MVP scope item, each mapped to the atomic capabilities that would implement it (per [use-case-tool-mapping-brainstorm.md](../docs/developer/use-case-tool-mapping-brainstorm.md)).

### Document publishing (charter, vision, roadmap)
- Author charter/vision/roadmap as Notion pages; publish via Notion Sites for public read access.
- Uses: Notion markdown read/write, public page/site publishing.

### Basic info publishing (officers, links, contact info)
- A published Notion page (or database view) listing current officers, roles, contact info, and links to social/web presence.
- Uses: Notion database CRUD, public page/site publishing.

### Conduct a meeting
- **Agenda creation**: Notion page/database record per meeting, built from a recurring agenda template.
- **Attendance**: Notion database record (or property on the meeting record) capturing who attended, linked to CRM contact records.
- **Note-taking**: Notion's native AI meeting notes (summary/notes/transcript) captured against the meeting page.
- **Briefing document links**: relations/links from the meeting record to supporting Notion pages or Google Docs.
- **Decision records**: Notion database properties on the meeting record (motion text, mover/seconder, result) — same low-volume, human-legible fit established earlier.
- Uses: Notion database CRUD, AI meeting notes, relations/rollups, markdown read/write.

### Minutes archive
- Published, searchable Notion database of past meeting records (one row per meeting, linking to full notes/decisions), each optionally exposed via Notion Sites for public transparency.
- Uses: Notion database CRUD, public page/site publishing.

### Committee participation interest form
- Public-facing form capturing name/contact/interest details, writing into the CRM as a new "occasional participant" or lead record.
- Uses: Notion native forms (or Airtable form, depending on which system owns the resulting CRM record), Airtable record CRUD.

### CRM with contact types (formal members, occasional participants)
- Airtable base with a `Contact Type` field (formal member / occasional participant, extensible later) and linked records for interactions, attendance, and subscription/consent status.
- Uses: Airtable relational schema, batch record CRUD, attachments (for signed documents if needed later).

### Social content: drafting/posting, Facebook event, posting minutes
- Draft posts (meeting time/place, drip agenda-item posts) in Notion or Google Docs; Zapier publishes to Facebook on schedule.
- Facebook Event created per meeting via the Facebook Events API, triggered by the meeting record being created/published.
- Minutes posted to Facebook once published, via the same publishing automation.
- Uses: Zapier webhook-triggered automation + scheduled polling, Facebook page post publishing, Facebook Events API.

### Email: agenda in advance, drip reminders, interest/attendance links
- Agenda emailed ahead of the meeting via EmailOctopus, triggered by the meeting record's scheduled date (Zapier).
- Drip reminder sequence in EmailOctopus counting down to the meeting.
- Email contains "I'm interested" / "confirm attendance" links that write back into the CRM (Airtable) attendance/interest status via a webhook.
- Uses: EmailOctopus campaign send + automation sequences, Zapier webhook-triggered automation, Airtable record CRUD.

### Shared workspace for committee members
- A Notion workspace area for brainstorming/research: pages for research notes, an uploads/links database with metadata properties (source, date, tags), and a quotes-with-citations database (quote text, source link, citation).
- Uses: Notion database CRUD, relations/rollups, markdown read/write.

### Committee newsletter subscription sign-up
- Public sign-up form feeding directly into an EmailOctopus audience list (and/or the Airtable CRM subscription-preference field, kept in sync).
- Uses: Notion native forms (or a dedicated form), EmailOctopus contact/audience list management via API.

## UX theory

Grounded in researched Notion mechanics: teamspaces, sharing/permission levels, database views, and native forms (see references at the end of this section).

### Notion navigation scheme

One Notion workspace per committee (per the federated model). Within that single workspace:

- **Sidebar top level** — a small, fixed set of top-level pages/databases, acting as the primary navigation (Notion's sidebar shows teamspaces, then pages/databases within them):
  - 🏠 **Home** — a wiki-style landing page, also the page published as the Notion Site's homepage. Links out to each area below; carries the org's identity (icon/cover).
  - 📜 **Governance** — Charter, Vision, Roadmap as individual sub-pages (narrative content, not a database — see schema notes).
  - 🗓️ **Meetings** — the Meetings database. Default view is a Table (all fields, officer-facing); a Calendar view for scheduling; a filtered, read-only view (e.g., `Status = Published`) is what gets exposed on the public Notion Site as the **Minutes Archive**.
  - 🗂️ **Shared Workspace** — Research & Assets database (brainstorming pages, uploads/links with metadata, quotes with citations), for formal members only.
  - 👤 **Officers** (Basic Info) — a small Officers database/page; a filtered gallery view of it is what's published publicly as "who to contact."
  - 📥 **Get Involved** — two pages, each hosting a native Notion Form: "Participation Interest" and "Newsletter Signup."

- **Teamspace segmentation**: given a single small workspace per committee, one default (open) teamspace covering everyone is likely sufficient rather than multiple teamspaces — but a second, **closed** teamspace for pre-publication drafts (unapproved minutes, in-progress decisions) is worth considering if officers want a working-draft stage hidden from general members before publishing. This is a judgment call to make once real usage patterns are known, not a hard requirement.

- **Permission tiers**, mapped to Notion's actual access levels:
  - **Public visitors** (no Notion account): reach only what's explicitly published via Notion Sites — Home, Governance, the filtered Minutes Archive view, and the Officers gallery view. Configured via each page's `General access → Anyone on the web with link` (or the Notion Site publish flow), not workspace-wide sharing.
  - **Formal members** (workspace members): `Can edit content` on the Meetings and Research & Assets databases — can add agenda items/research notes/attendance but can't restructure schema, views, or filters.
  - **Officers**: `Full access` to Governance, Meetings, and Officers databases — can change schema, approve/publish records, and manage the workspace.
  - **Form respondents** (Get Involved forms): don't need a Notion account at all. Forms are shared with `Anyone on the web with link`; page-level access can be set so respondents only see their own submission (`Can create` avoids exposing other people's entries), consistent with Notion's documented pattern for public-intake databases.

### General schema

Deliberately thin and narrative-oriented on the Notion side — CRM data (contact types, interaction logs, subscription preferences) stays in Airtable per the data-placement principle established in agenda item 1; Notion holds only what's meant to be read or edited by humans, plus the minimal structured properties needed to drive the views above.

- **Governance pages** (not a database — a handful of narrative pages): Charter, Vision, Roadmap, each a standalone page under the Governance section.
- **Meetings** (database): `Title`, `Date`, `Status` (Draft / Scheduled / Held / Published), `Agenda` (sub-page or relation), `Briefing Links` (URL/relation to supporting docs), `AI Meeting Notes` (native `meeting_notes` block), `Decisions` (relation to the Decisions database), `Attendees` (a simple property for internal reference — full attendance/CRM tracking lives in Airtable).
- **Decisions** (database, related to Meetings): `Motion Text`, `Mover`, `Seconder`, `Result` (Passed/Failed/Tabled), `Meeting` (relation), `Date`.
- **Officers** (small database): `Name`, `Role/Title`, `Contact Info`, `Bio`, `Term Dates`, `Photo` — source for both the internal Meetings/Governance work and the published public Basic Info page.
- **Research & Assets** (database, Shared Workspace): `Title`, `Type` (Document / Link / Upload / Quote), `Source URL`, `Tags`, `Date Added`, `Added By`, plus `Quote Text` and `Citation` properties for quote-type entries.
- **Get Involved forms**, each backed by its own small database: **Participation Interest** (`Name`, `Contact`, `Areas of Interest`, `Notes`) and **Newsletter Signup** (`Name`, `Email`) — both wired via Zapier to also write a corresponding record into the Airtable CRM, which remains the system of record for contacts.

**Recipe-library implication**: each grouping above (Governance pages, Meetings+Decisions, Officers, Research & Assets, Get Involved forms) is a natural candidate for its own recipe — a self-contained schema an agent could provision via Notion's schema-management API when setting up a new committee instance, consistent with the recipe-library model from agenda item 2.

References: [Teamspaces](https://www.notion.com/help/intro-to-teamspaces), [Sharing & permissions](https://www.notion.com/help/sharing-and-permissions), [Views, filters, sorts & groups](https://www.notion.com/help/views-filters-and-sorts), [Forms](https://www.notion.com/help/forms).
