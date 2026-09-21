# Notion / Automation Architecture — Brainstorm

Working notes only. Not yet promoted to `manifest/`. See [working-doc-promotion.md](docs/developer/approach/working-doc-promotion.md) for promotion criteria.

## Brain-dump recap (2026-09-20)

- Notion recommended by CIMA for "AI note taking"; author has only light prior exposure (task manager, years ago). No familiarity yet with CIMA's specific Notion AI features or templates.
- Working assumptions about Notion: solid document store, cross-doc search, taxonomy (databases/properties), public sharing via canonical URLs, integrations with other platforms.
- Zapier: integrate via **API exclusively** (not the no-code UI builder) to automate workflows.
- Google Docs: real-world target orgs (volunteer committees) currently use Google Docs. Even if Notion becomes primary, want to keep Google Docs supported — at minimum, keep it synchronized as a **publishing endpoint** from Notion.
- BI/data analytics: seen as a stretch for this domain, but possible narrow use-case — social media campaign engagement reporting/analysis.
- CRM-style contact tracking floated as a candidate for a data store **outside** Notion.
- Tentative platform set: **Notion, Zapier, Airtable, possibly Bubble.**

## Reactions

**Notion assumptions — mostly right, worth sharpening**
- Databases + properties (relations, rollups, formulas) *are* the taxonomy layer — this is the piece to demo, not just "search."
- Public share links are real (canonical URLs, can disable duplication/editing) — good fit for a governance solution that needs to publish minutes/decisions externally.
- Notion API (2023+) supports pages, databases, blocks, comments, and now Notion AI-adjacent features (Q&A) are largely UI-only, not API-exposed yet — don't over-promise API-driven AI features in the demo.

**Zapier via API only**
- This is a stronger, more defensible engineering choice than it may seem: it's version-controllable, testable, and reviewable (vs. a UI Zap that's hard to diff/audit). It also directly mirrors CIMA's own roadmap language — their **Language Library** project explicitly plans a "Bridge" automation "using Make/Zapier." Building this with API-driven, inspectable logic is a good discussion point on engineering discipline vs. no-code sprawl.

**Google Docs as a supported publishing endpoint**
- This is the single strongest alignment with CIMA's *own* internal architecture. Their Language Library pattern is: consultant tags a Google Doc → automation creates a Notion record → Notion becomes the searchable index, Google Doc stays the source of truth for editing. A volunteer-committee analog (e.g., meeting minutes drafted in Google Docs, indexed/published via Notion) demonstrates you understood their existing pattern and can generalize it to a new domain — likely to land well in Section 1 discussion too.

**BI / data**
- Agreed that full BI tooling is a stretch. Campaign engagement reporting is a reasonable narrow use case, but likely servable with a Notion database + native chart/rollup views, or a lightweight embedded chart, rather than introducing a dedicated BI tool. Keep this as a "could extend later," not core scope.

**CRM/contact-tracking outside Notion**
- Reasonable instinct. Notion databases can do lightweight CRM (relations/rollups), but volume, dedupe, and view-heavy CRM workflows are where Airtable or a real CRM outperform Notion. Worth deciding explicitly: is contact-tracking in-scope for the *demo*, or just architecturally anticipated?

**Platform set: Notion + Zapier + Airtable + (maybe) Bubble**
- Notion + Zapier (API) are clearly justified by the use-cases above.
- Airtable is justified *if* CRM/contact-tracking or structured campaign data is in scope; otherwise it may be redundant with Notion databases — worth pressure-testing before committing, especially given "keep it simple."
- Bubble is the one to interrogate hardest: it implies a custom-built user-facing app/portal. Is there an actual use-case that Notion's native sharing + forms + Zapier can't satisfy (e.g., a public intake form, a voting/motions tool, a member portal)? If not, Bubble should stay a labeled stretch/extensibility item rather than core architecture — both for scope discipline and for a cleaner 10-minute demo narrative.

## Interview-fit considerations

- Section 3 of the assignment explicitly asks for: discovery/pain points, stakeholders/skeptics, alternatives evaluated, pivots, and a technical walkthrough (data architecture, automation logic, UI/UX). The Notion↔Google Docs↔Zapier spine gives a clean story for all five.
- Directly mirroring a pattern from CIMA's *own* internal roadmap (Language Library's Google Doc → tag → Notion record bridge) is a strong signal for Section 1 (architecting CIMA's own tools) — it shows you can read their roadmap and generalize the pattern to a client-facing problem.
- Given the 10-minute format and the "keep it simple" instruction, recommend scoping the *demoed* build to Notion + Zapier (API) + Google Docs sync as the core, and presenting Airtable/Bubble as clearly-labeled extensibility options rather than building all four platforms out.

## Open questions to resolve before drafting the Manifest

1. Is CRM/contact-tracking in-scope for the demo, or noted as future extension?
2. What specific volunteer-committee governance use-case(s) will anchor the demo (e.g., mandate/bylaws, recruitment, meeting minutes, motions/voting, reporting)?
3. Does a public-facing portal/intake use-case exist that justifies Bubble, or is Notion public sharing sufficient?
4. What does "keeping Google Docs supported" mean precisely — read-only sync of published Notion content, or bidirectional editing?

## Notion vs. Airtable — data placement, APIs, IaC

**Where should machine-oriented data live?**
Not in Notion. Notion databases render as human-facing pages; high-volume, low-review data (activity logs, social/email interaction events, engagement analytics) degrades view performance and buries the content people actually read. Split responsibility: raw/event data lives in a system built for volume (Airtable or similar); Notion holds only aggregated, human-relevant summaries (e.g., a weekly rollup row), pushed in by automation.

**Airtable's general advantages**
- Real relational primitives (linked records, lookups, rollups, typed fields) vs. Notion's document-first model.
- Native automations reduce round-trips through Zapier for simple triggers.
- Higher practical row-volume ceiling before performance degrades.
- Batch API writes (up to 10 records/call).
- Interfaces (Airtable's app-builder) can substitute for a custom Bubble app in simple cases — a reason to defer Bubble.

**Notion API capabilities** ([developers.notion.com](https://developers.notion.com/reference/intro))
- REST API over pages, databases/data sources, blocks, users, comments, search; schema (properties/types) is createable and patchable via API.
- Query endpoint supports filter/sort with cursor pagination (100/page); ~3 req/sec average rate limit.
- No granular field-level webhooks and no automation layer as capable as Airtable's — reinforces the plan to drive automation externally via Zapier (API-only).

**Airtable API capabilities** ([airtable.com/developers/web/api](https://airtable.com/developers/web/api/introduction))
- REST/JSON API with a metadata (schema) API for tables/fields, plus official/community client libraries.

**IaC verdict**
Both platforms expose schema-management endpoints (Notion: database/data-source property schema; Airtable: base/table/field metadata API), but neither has a mature, first-party Terraform provider or drift-detection tooling. Practical approach for either: keep schema definitions as declarative files in the repo and write a small idempotent reconciliation script per platform, scoped to **schema only** — page/record content is not a good IaC target.

**Leaning decisions**
- CRM/contact-tracking and interaction-analytics data: outside Notion (Airtable or similar), summarized into Notion.
- Schema-as-code via bespoke reconciliation scripts, not Terraform, for both Notion and Airtable.
