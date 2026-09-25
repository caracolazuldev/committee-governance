# MVP approach

How the committee-governance **candidate MVP** (the full hypothetical scope from [missives/2026-09-20-pseudo-discovery-use-cases-ux-theory.md](../../../missives/2026-09-20-pseudo-discovery-use-cases-ux-theory.md), not the exploratory sprint) applies [committee-platform-architecture.md](../architecture/committee-platform-architecture.md) to specific implementation decisions, tradeoffs, and sequencing. Final MVP scope and implementation requirements remain subject to evidence from the [exploratory implementation sprint](exploratory-sprint.md). Facebook is outside that initial sprint.

## Decisions

| MVP area | Choice | Tradeoff / rejected alternative |
|---|---|---|
| Governance publishing (charter, vision, roadmap) | Notion pages, published via Notion Sites | Rejected Google-Docs-only: no built-in taxonomy, search, or canonical public URL |
| Basic info publishing (officers, links, contact) | Notion Officers database, filtered gallery view published publicly | N/A — direct application of the data-placement rule (human-facing → Notion) |
| Meeting lifecycle (agenda, attendance, notes, decisions) | Notion Meetings + Decisions databases; transcript source is an **open choice** (native Notion AI Meeting Notes vs. bring-your-own transcription) | Native Notion transcription costs a Business-plan seat regardless of recording method (see [notion-ai-meeting-notes-briefing.md](../notion-ai-meeting-notes-briefing.md)); bring-your-own avoids that cost but requires building the condense-into-minutes step ourselves |
| Minutes archive | Filtered, published view of the Meetings database | N/A |
| Participation interest form / newsletter sign-up | Notion native forms, `Can create` page-level access so respondents can't see others' entries; writes mirrored into Airtable/EmailOctopus via Zapier | Rejected a dedicated form tool: Notion forms are sufficient and keep the surface count low |
| CRM (formal member / occasional participant contact types) | Airtable base, `Contact Type` field, extensible | Rejected Notion-only CRM: clutters human-facing views, thinner relational model at volume |
| Social content drafting + Facebook posting/events | Draft in Notion/Google Docs; Zapier (API-only) publishes to Facebook Graph API and creates Facebook Events | Rejected Zapier's no-code UI builder: not version-controlled/reviewable |
| Email (agenda, drip reminders, RSVP links) | EmailOctopus, Zapier-orchestrated, writing RSVP/interest responses back into Airtable | Explicitly excluded from the exploratory sprint; included here because this is the full MVP scope |
| Shared member workspace (research, uploads, quotes/citations) | Notion Research & Assets database | N/A |
| Recipe provisioning | Each schema grouping (Governance pages; Meetings+Decisions; Officers; Research & Assets; Get Involved forms) is one recipe, fetched from the canonical Notion-hosted recipe library via its markdown API | Recipes are applied once per instance, not kept in a live-synced relationship (see architecture) |
| Bubble | Not adopted for the MVP | No confirmed use-case yet that Notion/Airtable native sharing and forms can't satisfy |
| Notion Agents (native Slack/Mail/Calendar automation) | Not adopted for the MVP | Unconfirmed whether Custom Agents are scriptable via the public API; defaulting to Zapier-driven automation for consistency/testability until resolved |

## Sequencing

Candidate dependency-ordered stages for the MVP (not approved or sprint-numbered; the exploratory sprint will inform final scope):

1. **Governance publishing + Basic info** — Notion pages/site, Officers database. Foundational; no dependencies on other stages.
2. **Meeting lifecycle** — Meetings + Decisions databases, minutes archive. Depends on Stage 1's Notion schema/publishing conventions. Transcript-source decision (native vs. bring-your-own) must be settled before this stage completes.
3. **CRM + Get Involved forms** — Airtable base, participation interest form, newsletter sign-up. Can proceed in parallel with Stage 2; forms need the Airtable CRM schema to exist first.
4. **Social/Facebook announce loop** — depends on Stage 2 (needs published minutes/announcements to post) and the Zapier orchestration pattern proven during the exploratory sprint.
5. **Email (EmailOctopus)** — depends on Stage 3 (CRM/subscription data as the audience source) and reuses the Zapier orchestration pattern from Stage 4.
6. **Shared workspace** — Research & Assets database. Lowest dependency; can slot in opportunistically alongside any other stage.
7. **Recipe-library formalization** — packaging each stage's schema grouping as a fetchable recipe. Follows, rather than precedes, each stage's schema stabilizing.

## Open decisions carried from discovery (not resolved by this approach)

- Native Notion transcription vs. bring-your-own transcription (cost vs. engineering-effort tradeoff).
- Notion Agents vs. custom Zapier automation for recap distribution and action-item handling.
- Whether a parent-organization cross-committee rollup mechanism gets built at all.
- Whether votes/decisions need audit-trail rigor beyond each platform's native version history.

## Links

- Business requirements: `manifest/business-requirements.md` (still placeholder — pending promotion from this discovery)
- Architecture: [committee-platform-architecture.md](../architecture/committee-platform-architecture.md)
- Discovery: [missives/2026-09-20-pseudo-discovery-use-cases-ux-theory.md](../../../missives/2026-09-20-pseudo-discovery-use-cases-ux-theory.md)
- Source brainstorms: [notion-architecture-brainstorm.md](../notion-architecture-brainstorm.md), [problem-space-brainstorm.md](../problem-space-brainstorm.md), [use-case-tool-mapping-brainstorm.md](../use-case-tool-mapping-brainstorm.md)
- Meeting record: [missives/2026-09-20-architecture-brainstorm-meeting-notes.md](../../../missives/2026-09-20-architecture-brainstorm-meeting-notes.md)
- Live work: `ROADMAP.md`
