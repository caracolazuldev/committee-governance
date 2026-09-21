# Use-Case → CIMA Tool Mapping — Brainstorm

Working notes only. Not yet promoted to `manifest/`. See [working-doc-promotion.md](../docs/developer/approach/working-doc-promotion.md) for promotion criteria.

Maps the problem-space catalog ([problem-space-brainstorm.md](problem-space-brainstorm.md)) against the tool set explored in agenda item 1 ([notion-architecture-brainstorm.md](notion-architecture-brainstorm.md)): Notion, Zapier (API-only), Airtable, Google Docs, and (deferred) Bubble.

## Mapping framework (recap of established principles)

- **Notion** — human-facing narrative/structured content: governance records, taxonomy, publishing, the recipe library (markdown API, human + agent readable).
- **Airtable** — structured/relational/high-volume or sensitive data: CRM entities, logs, anything that would clutter Notion's human-facing views.
- **Google Docs** — long-form co-authored drafting and a publishing endpoint for orgs already using it; content gets tagged/indexed into Notion (mirrors CIMA's Language Library pattern).
- **Zapier (API-only)** — automation glue between systems; version-controlled, not the no-code UI builder.
- **Bubble** — deferred; only if a confirmed use-case needs a custom-built portal beyond Notion/Airtable's native sharing and forms.
- **Google Calendar** (new) — scheduling/availability and the target for published event data.
- **EmailOctopus** (new) — outbound email delivery (newsletters, onboarding sequences, campaign sends).
- **Facebook** (new) — social publishing/events and a source of public engagement data.

## Atomic technical capabilities by platform

Abstracting away from specific use-cases: the underlying technical capabilities each platform needs to provide, with example use-cases as justification (not an exhaustive use-case list — see the theme-by-theme mapping above/below for that).

### Notion

- **Database record CRUD via API** — create/read/update pages within a data source.
  - Minutes and vote/decision records; event records; feedback-triage records; CRM aggregate-summary rollups.
- **Schema/property management via API** (create/patch database properties) — declarative, scriptable schema setup.
  - Schema-as-code application of a recipe (e.g., "Minutes & Votes" recipe provisions its database schema in a new committee instance).
- **Markdown read/write** (`GET/POST/PATCH .../markdown`) — content as text, not just structured properties.
  - Recipe-library consumption by an agent; drafting/publishing governance narrative pages (charter, roadmap, SWOT).
- **AI meeting notes** (`meeting_notes` block: summary/notes/transcript) — native transcription pipeline.
  - Meeting minutes with AI transcription (hard requirement).
- **Public page/site publishing** (Notion Sites) — one-click public web presence per page.
  - Publishing minutes, roadmaps, and progress reports for transparency; a public-facing events calendar page.
- **Native forms** — structured public data intake without a separate tool.
  - Surveys; event registration/sign-up; public feedback intake.
- **Relations & rollups** — cross-reference and aggregate within a workspace.
  - Linking action items back to their originating minute; aggregating CRM summary counts sourced from Airtable.
- **Workspace member roles / guest sharing** — access tiering.
  - Volunteer vs. officer edit access; public read-only sharing of specific pages.

### Zapier (API-only)

- **Webhook-triggered automation** — an event in one system triggers a workflow.
  - New Notion minutes record triggers a Google Calendar event update; a new Airtable volunteer record triggers a welcome sequence.
- **Scheduled polling automation** — time-based triggers independent of a live event.
  - Weekly check of Airtable for training/certification renewals due; periodic re-fetch of a recipe page to check for updates.
- **Multi-step workflow orchestration** — chaining several app actions from one trigger.
  - Volunteer onboarding: create Airtable record → send welcome email via EmailOctopus → post prerequisite doc links.
- **Code/transformation step** — reshaping data between apps with custom logic.
  - Converting a Notion markdown recipe/page into a formatted Facebook post or EmailOctopus campaign body.
- **Programmatic Zap deployment** (Zapier Platform API/CLI, not the UI builder) — version-controlled, reviewable automation definitions.
  - Reproducing the same automation set across multiple committee instances as part of applying a recipe.

### Airtable

- **Relational schema** (linked records, lookups, rollups) — modeling entities and relationships, not just flat rows.
  - CRM: volunteer ↔ organization ↔ interaction linking; power-mapping network (people, organizations, relationships).
- **Batch record CRUD via REST API** — bulk programmatic writes.
  - Bulk import of prospects/volunteers; bulk logging of interaction/engagement events.
- **Views as queryable filters** (grid/kanban/calendar) — structured slices of the same base data.
  - Training-expiration dashboard; event-calendar staging view before publishing out to Google Calendar/Facebook.
- **Native automations** (in-base triggers/actions) — simple automations without a Zapier round-trip.
  - Auto-flagging a high-priority interaction for follow-up.
- **Attachments field** — file storage tied to a structured record.
  - Signed code-of-conduct or consent-form scans tied to a volunteer record.
- **Metadata/schema API** (create/update tables and fields programmatically) — schema-as-code, same pattern as Notion's.
  - Recipe-driven provisioning of a CRM base's table/field structure for a new committee instance.

### Google Docs

- **Long-form co-authored drafting** — real-time multi-editor text authoring Notion doesn't optimize for.
  - Charter/bylaws drafts; SOPs; translated newsletter copy.
- **Comment/suggestion review workflow** — native editorial review before publishing.
  - Governance document review before promotion into Notion.
- **Read via Google Docs API for tagging/indexing** — bridging drafted content into the searchable knowledge base.
  - Language-Library-style bridge: tag a finished doc, auto-file a linked record into Notion.

### Google Calendar (new)

- **Event CRUD via API** — programmatic scheduling.
  - Publishing the committee meeting schedule; public event calendar entries.
- **Free/busy & availability queries** — scheduling coordination.
  - Finding a quorum-friendly meeting time across officer availability.
- **Public calendar sharing/embedding** (including iCal/RSS-style feed) — a public-facing subscribable calendar.
  - Transparency requirement: publish a subscribable events calendar alongside the Notion Site.
- **Reminders/notifications tied to events** — time-based nudges.
  - Meeting reminders; renewal/expiration deadlines surfaced alongside calendar entries.

### EmailOctopus (new)

- **Contact/audience list management via API** — subscriber list as a first-class object.
  - Mirroring Airtable's CRM subscription-preference data into a sendable list.
- **Campaign send via API** — triggering an actual email send programmatically.
  - Newsletter distribution; volunteer onboarding welcome email.
- **Automation sequences** (drip campaigns) — multi-step, time-delayed email flows.
  - Volunteer onboarding sequence; post-event follow-up sequence.
- **Engagement tracking/webhooks** (opens/clicks) — feeding analytics back into the system.
  - Campaign-engagement reporting/analysis use-case, landing in Airtable rather than Notion.

### Facebook (new)

- **Page post publishing via API** (Graph API) — programmatic social posting.
  - Scheduled social media posts drafted in Notion/Google Docs, published via Zapier.
- **Events API** (create/publish Facebook Events) — one more calendar-publishing target.
  - Multi-platform event calendar publishing alongside Google Calendar/RSS.
- **Engagement/insights retrieval** (comments, reactions, post insights) — a source of analytics data.
  - Social media campaign engagement reporting/analysis use-case.
- **Webhooks for page activity** (new comment/message) — inbound public engagement.
  - Public feedback triage inbound channel.

## Theme-by-theme mapping

### Governance & democratic process
- Charter/mandate, SWOT outputs, roadmaps: **Notion** pages with version history; **Google Docs** as an optional co-drafting stage before publishing into Notion.
- Minutes, motions/votes/decisions: **Notion database** (structured properties: motion text, mover/seconder, result, date) — low-volume, human-legible, a strong native fit.
- Elections/term limits, bylaws compliance, grievance records, retention policy: **Notion databases/pages** — still human-facing and comparatively low-volume.
- Multi-committee/federated roll-up reporting: **open problem** — Notion has no native cross-workspace query, so a parent-org rollup across committee instances needs a separate aggregation mechanism (candidate: a scheduled Zapier/script job that reads each committee's Notion via its own connection and writes a summary somewhere central — Airtable or a parent-org Notion workspace).

### Meetings & institutional knowledge
- AI transcription: **Notion's native AI meeting notes** (`meeting_notes` block, API-exposed) as the default candidate; external tool + Zapier bridge as a fallback if native coverage is insufficient.
- Agenda templates, action-item tracking: **Notion database** with recurring-item templates.
- Succession/handoff packets: **Zapier**-assembled summary pulling from Notion's own records (query recent minutes/action items into a generated packet page).

### CRM & relationship management
- Volunteers/consultants, households/minors, skills/availability, membership, donor relationships, subscription preferences: **Airtable** — relational primitives, higher volume, and sensitive data that shouldn't clutter Notion's human-facing views. Notion holds only aggregated summaries (e.g., "12 active volunteers, 3 onboarding").
- Interaction/consent audit trail: **Airtable**, same reasoning — structured, potentially high-volume, and tied to compliance/safeguarding needs (minors) that argue for a stricter data model than Notion pages provide.

### Community organizing
- Power-mapping, coalition/stakeholder mapping: **Airtable** for the underlying network data (organizations, people, relationships, influence scores) with **Notion** narrative pages for strategy interpretation and write-ups.
- Campaign/issue tracking, base-building metrics: **Airtable** for structured tracking; **Notion** for narrative campaign retrospectives.

### Outreach, campaigns & communications
- Social/email drafting and scheduling: drafts in **Google Docs** or **Notion**; **Zapier** orchestrates scheduled posting/sending to the actual channels (social platforms, ESP).
- Multi-channel campaign calendars: **Airtable** as the structured calendar/database; **Zapier** triggers the sends.
- Newsletter management: **Google Docs** (drafting) → **Notion** (index/archive, mirroring the Language Library tag-and-file pattern) → **Zapier** (distribution).
- Press/crisis-communication kit: **Notion** pages (some public via Notion Sites, some internal-only).
- Translation/multilingual workflow: same Google Docs↔Notion bridge pattern as the Language Library, applied to translated variants.

### Volunteer onboarding & lifecycle
- Onboarding automation (welcome email, doc links, prerequisites): **Zapier** orchestrates the sequence; status tracked in **Airtable** (or a lightweight Notion database if volume stays low); source documents live in **Notion**.
- Training/certification tracking (expirations/renewals): **Airtable** — structured, date-driven, needs reminders (via Zapier).
- Offboarding: **Zapier**-triggered checklist against **Airtable** volunteer records.
- Recognition/gamification: **Notion** for a public-facing recognition page; **Airtable** for the underlying hours/milestones ledger.

### Events & calendars
- Multi-platform calendar publishing (Facebook, calendar services, RSS): event records held in **Airtable** or **Notion database**; **Zapier** bridges to each target platform's API.
- Registration/sign-up: **Notion Forms** (Notion's native form feature) or **Airtable** forms feeding into the CRM/attendee base.

### Communications asset library
- Photos/video links, taxonomy, license/attribution tracking: **Notion database** — taxonomy via properties is a Notion strength, and this is comparatively low-volume, human-browsed content.
- Rights/consent tracking for photos of individuals (especially minors): worth a stricter, audited model — likely **Airtable** if volume/compliance needs outgrow a simple Notion property.

### Public engagement tooling
- Surveys and public forms: **Notion Forms** or **Airtable** forms depending on which system needs to own the resulting records.
- Feedback triage and closed-loop response: **Notion database** with a status workflow (received → reviewed → responded).

### Cross-cutting / platform-level themes
- Roles & permissions: Notion has native workspace member roles/guest sharing; whether that's sufficient for volunteer/officer/public-visitor tiers, or whether a custom layer is needed, is still open.
- Budget/treasury: **Airtable** for structured financial data; **Notion** for narrative budget-vs-actuals summaries.
- Notifications: **Zapier**-orchestrated, multi-channel (email/SMS).
- Audit trail/security: Notion has page version history; Airtable has revision history on paid plans; neither is obviously compliance-grade for legally consequential records (votes/decisions) — flagged as open.
- Data portability: both platforms support export (Notion export, Airtable CSV/API) — no blocker identified yet.
- Offline/low-bandwidth access: unaddressed by this tool set; both Notion and Airtable are cloud-only.
- Multi-tenancy: resolved at the architecture level by the federated model — one Notion workspace (and one Airtable base, if used) per committee, not shared infrastructure.

## Open questions

1. Is the cross-workspace rollup problem (parent org viewing multiple committee instances) solved with custom aggregation tooling, or is it out of scope for now?
2. Do Notion's native workspace roles cover the volunteer/officer/public-visitor permission model, or is a custom layer needed?
3. Are votes/decisions "legally consequential" enough to require an audit trail Notion/Airtable's built-in history doesn't provide?
4. Where's the line between "low-volume enough for Notion" and "needs Airtable" for the asset-library rights/consent data — is there an actual volume estimate to test this against?
5. Offline/low-bandwidth access: acknowledge as a known gap, or actively design around it?
