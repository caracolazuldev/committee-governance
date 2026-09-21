# Meeting Notes — Architecture Brainstorm

**Date:** 2026-09-20
**Attendees:** Maintainer, Number One (agent, meeting secretary)
**Purpose:** Raw discovery notes toward a Manifest document (business requirements, personas, glossary) per MKA.

## Agenda

1. Review CIMA desired platform tools — general purpose and strengths
2. Explore and articulate the problem space of "volunteer committee governance"
3. Map real-world use-cases to CIMA preferred tools
4. **(added mid-meeting)** Pseudo-discovery: specific use-cases + UX theory (Notion navigation scheme and general schema), using the use-case/tool mapping as a loose architectural guide and the problem-space brainstorm as source material
5. **(amended)** Exploratory scope: identify the most minimal set of functionality that exercises Notion, Zapier, Airtable, and Facebook together (email/EmailOctopus excluded for now)

## Notes

### Agenda item 1: CIMA desired platform tools — purpose & strengths

Reviewed candidate platform set (Notion, Zapier, Airtable, possibly Bubble) against CIMA's own tooling patterns. Full discussion in [docs/developer/notion-architecture-brainstorm.md](../docs/developer/notion-architecture-brainstorm.md).

- **Notion**: document store + taxonomy (databases/properties) + public share links + integrations. Strong fit as the human-facing index and publishing surface; not built for high-volume/machine-oriented data (rate limit ~3 req/sec, no granular webhooks, no automation layer of substance).
- **Zapier (API-only)**: mirrors CIMA's own planned "Bridge" automation pattern (Language Library project); chosen for version-controllability/testability over the no-code UI builder.
- **Google Docs**: to remain a supported publishing endpoint alongside Notion — directly analogous to CIMA's Google Doc → tag → Notion record pattern.
- **Airtable**: better suited than Notion for relational/high-volume structured data (CRM contacts, interaction/engagement logs) due to native relational primitives, batch API writes, and native automations. Machine-oriented data (activity logs, social/email interaction events, analytics) should live in Airtable (or similar), not Notion, to avoid cluttering human-facing views; Notion holds only aggregated summaries.
- **Bubble**: deferred as a stretch/extensibility item, not core — no confirmed use-case yet that Notion sharing/forms can't satisfy. Airtable Interfaces may cover simple app-like needs instead.
- **IaC**: both Notion and Airtable expose schema-management APIs (no mature Terraform providers for either). Plan: declarative schema files + bespoke reconciliation scripts per platform, scoped to schema only, not content/records.

References: [Notion API docs](https://developers.notion.com/reference/intro), [Airtable API docs](https://airtable.com/developers/web/api/introduction).

### Agenda item 2: problem space of "volunteer committee governance"

Brain-dumped and expanded into a comprehensive, non-scope-limited catalog. Full discussion in [docs/developer/problem-space-brainstorm.md](../docs/developer/problem-space-brainstorm.md).

- Target user: volunteer-run, resource-constrained community organizations, with inexperienced volunteers needing guidance on democratic governance/collaboration best practice.
- Catalog spans: governance & democratic process (charter, votes, elections, bylaws compliance, grievance process, retention policy), meetings & institutional knowledge (AI transcription, agenda-builder, succession packets), CRM & relationship management (volunteers, households/minors, skills/availability, membership, donors), community organizing (power-mapping, coalition mapping, campaign tracking), outreach/campaigns/communications, volunteer lifecycle (onboarding through offboarding, recognition), events & calendars, communications asset library, and public engagement tooling (surveys, forms, feedback loop closure).
- Cross-cutting platform themes identified: roles/permissions, accessibility/compliance, budget/treasury, notifications, audit trail/security, data portability, offline access, multi-tenancy.
- **Meta-framework theme**: the deliverable may be a knowledge-base + solution-recipe library (catalog of implementable capabilities an org/committee selects from), with a **federated model — one Notion instance per committee**, not per parent organization.
- Recipe-consumption model: recipes live in Notion (human-editable) and are also machine-readable — confirmed Notion's markdown API (`GET/POST/PATCH .../markdown`) lets an agent fetch a recipe as clean markdown and apply it once into a committee instance, with no live sync back to the source (distinct from mka-bootstrap's own template-update remote pattern, which pulls updates continuously).
- Cross-workspace read access (agent in a committee workspace reading the shared recipe-library workspace) remains an open architecture item — Notion API access is scoped per-workspace.

References: [Notion markdown API docs](https://developers.notion.com/guides/data-apis/working-with-markdown-content).

### Agenda item 3: mapping use-cases to CIMA preferred tools

Mapped the problem-space catalog against the tool set theme-by-theme, then abstracted to a platform-by-platform list of atomic technical capabilities (with justifying use-cases) for Notion, Zapier, Airtable, Google Docs, and three newly added integrations — Google Calendar, EmailOctopus, and Facebook. Full discussion in [docs/developer/use-case-tool-mapping-brainstorm.md](../docs/developer/use-case-tool-mapping-brainstorm.md).

- Theme-by-theme mapping confirmed the Notion/Airtable data-placement split from item 1 holds across the full catalog (governance/meetings/comms → Notion; CRM/organizing/logs → Airtable).
- Added atomic-capability inventories per platform (e.g., Notion: database CRUD, schema management via API, markdown read/write, AI meeting notes, public site publishing, native forms, relations/rollups, workspace roles; similarly enumerated for Zapier, Airtable, Google Docs, Google Calendar, EmailOctopus, Facebook).
- Flagged several open problems: cross-committee rollup reporting (no native cross-workspace query in Notion), whether Notion's native roles cover the volunteer/officer/public tiering need, and whether votes/decisions need audit-trail rigor beyond Notion/Airtable's built-in history.
- This brainstorm thread will likely be revisited later in the process; not closed permanently, just paused here.

**Agenda amendment**: inserted a new item 4 — a "pseudo-discovery" document (in `missives/`, not `docs/developer/`) that uses this tool mapping as a loose architectural guide and the problem-space catalog as source material to produce specific use-cases and a UX theory, including a Notion navigation scheme and general schema.

### Agenda item 4: pseudo-discovery — use-cases & UX theory

Produced specific MVP-scope use-cases and a first UX theory. Full discussion in [missives/2026-09-20-pseudo-discovery-use-cases-ux-theory.md](2026-09-20-pseudo-discovery-use-cases-ux-theory.md).

- Established a hypothetical **MVP scope** (larger than the exploratory implementation): document/basic-info publishing, conducting a meeting (agenda/attendance/notes/decisions), minutes archive, participation interest form, lightweight CRM (formal member / occasional participant), social content drafting + Facebook posting/events, email agenda/reminders/RSVP links, a shared member workspace, and newsletter sign-up.
- Each MVP item mapped to concrete atomic capabilities (per agenda item 3's inventory).
- **UX theory**: one Notion workspace per committee; fixed sidebar navigation (Home, Governance, Meetings, Shared Workspace, Officers, Get Involved); permission tiers mapped to Notion's real access levels (`Full access` officers, `Can edit content` members, public via page-level web sharing, form respondents via `Can create`); a thin, narrative-oriented Notion schema (Governance pages; Meetings/Decisions/Officers/Research & Assets databases; two Get Involved forms), with CRM data staying in Airtable per the established data-placement principle.
- Noted each schema grouping as a natural candidate for its own recipe in the recipe-library model.

References: [Notion Teamspaces](https://www.notion.com/help/intro-to-teamspaces), [Notion Sharing & permissions](https://www.notion.com/help/sharing-and-permissions), [Notion Views/filters/sorts/groups](https://www.notion.com/help/views-filters-and-sorts), [Notion Forms](https://www.notion.com/help/forms).

**Agenda amendment**: item 5 amended to "Exploratory scope" — rather than a full solution architecture proposal, identify the most minimal functionality set that exercises Notion, Zapier, Airtable, and Facebook together, with email/EmailOctopus explicitly excluded for now.

### Agenda item 5: exploratory scope

Proposed directly here (no separate brainstorm doc, per instruction) — the smallest closed-loop pipeline that touches all four required platforms, built around a real meeting lifecycle (agenda → meeting → transcript → minutes → publish → announce → measure) rather than a generic content post.

**Research finding — don't reinvent transcription**: Notion's own AI Meeting Notes is usable headlessly via API, not just the live desktop-app flow (`/meet`). `POST /v1/blocks/meeting_notes` accepts an uploaded audio/video file and creates a `meeting_notes` block that Notion transcribes and summarizes on its own — no custom transcription/summarization service needed. Processing is asynchronous: poll the block until `meeting_notes.status` reaches `notes_ready`, then read the generated `summary_block_id`, `notes_block_id`, and `transcript_block_id`. Requirements to note: the integration needs **Insert content** (and **Read content**) capability, and the workspace user must have AI Meeting Notes entitlement (Business/Enterprise plan, or an eligible AI-inclusive mobile subscription) — a real cost/plan constraint to flag, not just a technical one.

Pipeline:

1. **Create an agenda** — author a **Notion** Meetings-database record ahead of time with agenda items as page content — exercises Notion database record CRUD and markdown write.
2. **Hold the meeting** — record it by whatever means is convenient (conferencing tool recording, phone audio, etc.), producing an audio file. This step is outside Notion/Zapier — it's just "have an audio file when the meeting ends."
3. **Generate a transcript** — a **Zapier** automation uploads the audio file via Notion's File Upload API, then calls `POST /v1/blocks/meeting_notes` with the agenda page as parent, kicking off transcription + AI summary — exercises Zapier webhook/scheduled orchestration and Notion's native transcription (not custom-built).
4. **Update the agenda / condense into minutes** — once Zapier polls to `notes_ready`, it fetches the generated summary/notes and writes them back into the agenda page (or a linked Minutes record) via Notion's markdown update API — exercises Notion markdown read/update.
5. **Publish with a canonical URL** — the finished minutes page is published via Notion Sites (`Share → Publish`), producing a public `notion.site` URL for sharing — exercises Notion public page/site publishing (same mechanism established in agenda item 1).
6. **Announce** — the same Zap posts the published minutes link to **Facebook** as a Page post via the Graph API — exercises Facebook page post publishing.
7. **Measure** — a follow-up Zapier step retrieves engagement/insights on that Facebook post and logs it as a record in **Airtable** — exercises Facebook engagement/insights retrieval and Airtable record CRUD, and validates the earlier decision that this kind of machine-oriented data belongs in Airtable, not Notion.

This stays deliberately narrow (one meeting, one pipeline run) while now genuinely proving the transcription/minutes capability CIMA cares about, not just a generic publish-and-post loop. Email/EmailOctopus remains excluded for this pass.

References: [Take AI Meeting Notes in Notion](https://www.notion.com/help/ai-meeting-notes), [Create a meeting note (API)](https://developers.notion.com/reference/create-meeting-note).

**Correction — Notion Agents**: initial research undersold Notion's built-in capability. **Notion Agents** (the built-in Notion Agent, plus configurable Custom Agents) are a real, separate product feature — they run inside Notion and connect to **Slack, Mail, Calendar, and MCP integrations** natively, and can be configured (via Notion's own agent-builder, not the public API as far as confirmed) to pick up action items and distribute recaps automatically. This is what "the recap sends itself" / "your agent handles action items" actually refers to on Notion's marketing pages — a real feature, not fluff, but a **separate entitlement/cost** (Business/Enterprise, consumes "Notion credits") from AI Meeting Notes itself, and its automation is configured through Notion's UI/agent-builder rather than confirmed to be scriptable via the public REST API. Treated as a genuine **build-vs-use-native decision** for later: Notion Agents could replace our own Zapier steps for recap distribution and action-item follow-through, rather than us building that ourselves — worth evaluating before committing to the Zapier-only version of steps 4/6 above.

**Cost correction — recording location does not avoid the plan cost**: confirmed via [Notion pricing](https://www.notion.com/pricing) that "Meeting notes" (transcription + AI summary) is gated behind the **Business plan** ($20/seat/month, billed per paid workspace member) — Free/Plus only get a "Limited Trial." This gate applies to the entitlement itself, not the recording method: the API requirement is explicit that "the user associated with the integration must also have access to AI meeting notes," so uploading an externally recorded file via `POST /v1/blocks/meeting_notes` still requires the same Business-tier entitlement as the live desktop flow. Recording outside Notion does not reduce Notion-side cost by itself.

There are genuinely two architecture options here, not one:
1. **Use Notion's native transcription** (the pipeline above) — requires the workspace on Business plan; gets Notion's polished transcript, speaker labels, and AI summary "for free" (engineering-wise) in exchange for the plan cost.
2. **Bring your own transcription** — record the meeting anywhere, transcribe/summarize it with a separate, often free or cheap tool (e.g., built-in Zoom/Meet transcripts, a Whisper-based tool), and write the resulting plain markdown into a normal Notion page via the standard Public API — available on **every** Notion plan including Free. This avoids the Business-tier gate entirely, at the cost of losing Notion's native polish and needing to build the condensing step ourselves.

Given the target user is a resource-constrained volunteer committee, option 2 is likely the more realistic default for the MVP, with option 1 as a "nice to have if budget allows" upgrade path — not yet decided which one the exploratory pass should actually build.

## Decisions

- Machine-oriented/high-volume data (CRM contacts, social/email interaction logs, analytics) lives outside Notion; Notion holds human-facing summaries only.
- Zapier integration is API-driven exclusively.
- Bubble is deferred pending a confirmed use-case; not part of core architecture for now.
- Schema-as-code approach for Notion/Airtable: bespoke reconciliation scripts against declarative schema files, not Terraform.
- Problem-space exploration is intentionally not scope-limited; scoping/prioritization is deferred to a later agenda item.
- Recipe library lives in Notion (not a separate git corpus), leveraging Notion's markdown API for agent consumption alongside normal human editing.
- Federated model: one Notion instance per committee, not per parent organization.
- Agenda amended mid-meeting to add a pseudo-discovery item (specific use-cases + Notion UX theory) ahead of the exploratory-architecture item.
- Pseudo-discovery UX theory: one Notion workspace per committee, thin narrative-oriented schema, CRM stays in Airtable, permission tiers mapped to Notion's native access levels.
- Agenda item 5 amended to "Exploratory scope"; no brainstorm doc for this item — captured directly in minutes.
- Exploratory scope defined as a single Notion → Zapier → Facebook → Zapier → Airtable pipeline (author/publish/measure), with email/EmailOctopus excluded for now.
- Exploratory scope now built around a real meeting lifecycle (agenda → record → transcript → minutes → publish → announce → measure), using Notion's own AI Meeting Notes API (`POST /v1/blocks/meeting_notes`) rather than a custom-built transcription/summarization service.
- Confirmed Notion Agents (built-in Notion Agent + configurable Custom Agents) as a real, separate feature that natively connects to Slack/Mail/Calendar/MCP — not yet decided whether to use it in place of our own Zapier-driven recap distribution/action-item steps.
- Confirmed AI Meeting Notes is gated behind Notion's Business plan ($20/seat/month) regardless of recording method (live desktop or API-uploaded file) — recording outside Notion does not reduce this cost by itself; a "bring your own transcription" alternative (external transcription + plain markdown into Notion, any plan) remains a live, undecided option.

## Open questions

- Is CRM/contact-tracking in-scope for the interview demo, or noted as future extension?
- Which volunteer-committee governance use-case(s) anchor the demo?
- Is there a concrete use-case justifying Bubble?
- What does "Google Docs support" mean precisely — read-only publish sync, or bidirectional editing?
- Which catalog themes are core to the product vision vs. adjacent/future?
- How does a parent organization roll up reporting across federated per-committee instances?
- How does an agent in one committee's workspace get read access to the shared recipe-library workspace, given Notion's per-workspace API scoping?
- Is a "recipe was updated, should this instance reapply it" mechanism wanted, or intentionally out of scope?
- Cross-committee rollup reporting: custom aggregation tooling, or out of scope for now?
- Do Notion's native workspace roles cover the volunteer/officer/public-visitor permission model, or is a custom layer needed?
- Are votes/decisions consequential enough to require an audit trail beyond Notion/Airtable's built-in history?
- Is the officer-only closed teamspace for pre-publication drafts needed, or does the default open teamspace suffice initially?
- When (if ever) does EmailOctopus/email get added into the exploratory pipeline?
- Does the target Notion plan (Business/Enterprise, or an eligible AI-inclusive subscription) needed for AI Meeting Notes fit CIMA's/the demo's constraints?
- How is the meeting actually recorded/captured as an audio file for upload (conferencing tool export, phone recording, etc.) — left as a practical detail to settle during implementation, not an architecture blocker.
- Should Notion Agents (native Slack/Mail/Calendar automation) replace our own Zapier steps for recap distribution and action-item handling, or does the exploratory pass stay Zapier-only for consistency/testability? Also unconfirmed: whether Custom Agents are configurable/triggerable via the public API at all, or UI/agent-builder only.
- Does the exploratory/MVP build use Notion's native transcription (Business-plan cost) or a bring-your-own-transcription approach (any plan, more engineering, no native polish)?

## Adjournment

Meeting adjourned 2026-09-20. Agenda items 1–5 covered (item 5 amended mid-meeting to "Exploratory scope"; item 4 inserted mid-meeting). Open questions above carry forward to the next session.

## Follow-ups

- Digest into `manifest/business-requirements.md`, `manifest/personas.md`, `manifest/glossary.md` at meeting close
