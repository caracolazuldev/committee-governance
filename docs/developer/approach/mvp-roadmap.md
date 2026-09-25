# MVP roadmap

This document records candidate stages for the full committee-governance MVP described in [mvp-approach.md](mvp-approach.md). These stages are not approved delivery milestones; the [exploratory implementation sprint](exploratory-sprint.md) will validate capabilities and produce the implementation requirements used to set final MVP scope. Facebook is outside that initial sprint.

## Outcome

A committee can publish its governance and basic information, run a meeting lifecycle, collect participation interest, maintain contacts, announce through Facebook, send email, and collaborate on research and assets. Reusable recipes package the stable Notion schemas for provisioning new committee workspaces.

## Milestones

### Stage 1: Governance publishing and basic information

Deliver the canonical Notion pages for charter, vision, and roadmap; the public Notion Site; and the Officers database with its public filtered view.

**Depends on:** None.

**Done when:** Governance pages and officer information have agreed owners, a public URL, discoverable navigation, and a documented schema that later stages can reuse.

### Stage 2: Meeting lifecycle and minutes archive

Deliver Notion Meetings and Decisions databases, agenda and attendance capture, notes and decision records, and a public filtered minutes archive.

**Depends on:** Stage 1.

**Done when:** A meeting can move from agenda through attendance and notes to published minutes and linked decisions. The transcript source decision is recorded before acceptance.

### Stage 3: CRM and participation intake

Deliver the Airtable CRM with formal-member and occasional-participant contact types, plus Notion forms for participation interest and newsletter sign-up. Mirror responses through Zapier into Airtable and EmailOctopus.

**Depends on:** Stage 1. May proceed in parallel with Stage 2 after the shared conventions are accepted.

**Done when:** A respondent can submit an interest or subscription form without seeing other responses, and an authorized maintainer can trace the response into the appropriate audience record.

### Stage 4: Social announcement loop

Deliver reviewable Notion or Google Docs drafts and API-only Zapier automation for Facebook posts and Events.

**Depends on:** Stage 2 and the exploratory sprint's validated Zapier pattern. Facebook integration is a candidate for later MVP delivery, not part of the initial exploratory sprint.

**Done when:** An authorized maintainer can approve a meeting or governance announcement and produce the corresponding Facebook publication or Event with an auditable source link.

### Stage 5: Email communications

Deliver EmailOctopus audience integration and Zapier-orchestrated agenda, reminder, drip, and RSVP-link messages.

**Depends on:** Stage 3 and a validated automation pattern.

**Done when:** Subscription status is respected, messages use the correct audience, and RSVP or interest responses remain traceable to the CRM.

### Stage 6: Shared research and assets workspace

Deliver the Notion Research & Assets database for research notes, uploads, quotes, and citations.

**Depends on:** Stage 1 conventions; may proceed opportunistically alongside Stages 2 through 5.

**Done when:** Members can store and retrieve research artifacts with source context and consistent citation fields without exposing private material through public views.

### Stage 7: Recipe-library formalization

Package the stable schema groupings as fetchable recipes from the canonical Notion-hosted recipe library: Governance pages; Meetings and Decisions; Officers; Research and Assets; and Get Involved forms.

**Depends on:** Each corresponding schema milestone being accepted.

**Done when:** A recipe can be fetched through the documented markdown API and applied once to a new committee instance with no live-sync assumption.

## Cross-cutting acceptance

- Public views expose only information intended for public audiences.
- Private intake, CRM, and workspace material is access-controlled and not reachable through public views.
- Automation failures are visible to maintainers and can be retried without creating duplicate records or messages.
- Each accepted milestone has an owner, a short verification procedure, and an updated approach record.
- The MVP does not include Bubble, Notion Agents, parent-organization rollups, or enhanced vote audit trails unless an open decision changes scope.

## Open decisions and sequencing gates

1. Select native Notion transcription or bring-your-own transcription before the candidate meeting-lifecycle stage is scoped for delivery.
2. Resolve whether recap distribution and action-item handling use Notion Agents or custom Zapier automation before automations are expanded beyond the proven pattern.
3. Decide whether parent-organization rollups and stronger vote audit trails are MVP requirements before adding them to a milestone.

## References

- [MVP approach](mvp-approach.md)
- [Committee platform architecture](../architecture/committee-platform-architecture.md)
- [Business requirements](../../../manifest/business-requirements.md)
- [Live roadmap](../../../ROADMAP.md)
