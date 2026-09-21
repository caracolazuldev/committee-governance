# Problem Space — Volunteer Committee Governance

Working notes only. Not yet promoted to `manifest/`. See [working-doc-promotion.md](../docs/developer/approach/working-doc-promotion.md) for promotion criteria.

## Brain-dump recap (2026-09-20)

Raw list from the session, organized by theme (no content changed, just grouped):

- **Target user**: volunteer-run, community organizations — limited resources, no bespoke online presence, inexperienced volunteers needing guidance on democratic participation and governance/collaboration best practice.
- **Governance & transparency**: mandate/charter documents; concise progress reports; meeting minutes including official votes/decisions; publishing roadmaps; soliciting wider feedback on committee work; timely reports to supervising bodies (parent committee/board); SWOT exercises for mission/vision/values and roadmaps.
- **Meetings & institutional knowledge**: taking meeting minutes with AI transcription (hard requirement); building a document database for institutional knowledge continuity.
- **CRM**: cultivate consultants + volunteer pool; record interactions; model organizations; support power-mapping; maintain subscription/contact preferences; track onboarding and other journeys; track households and minors; track interests, capabilities/skills, availability.
- **Community organizing**: power mapping and other organizing best practices.
- **Outreach & campaigns**: plan outreach/engagement campaigns; draft and schedule social media posts and emails.
- **Volunteer onboarding automation**: welcome email, document links, prerequisites (signed code of conduct, completed profile).
- **Events & calendars**: draft/manage/publish event calendars to multiple platforms (Facebook, calendar services, RSS).
- **Communications asset library**: photos, links to photo/video services (YouTube, Vimeo, Flickr), taxonomy, license/attribution tracking.
- **Public engagement tooling**: surveys and public forms; event registration/sign-up.

## Reactions

**Note on framing (revised):** the first pass below over-indexed on demo/scope discipline. This agenda item is explicitly about exploring the problem space without constraining to a reasonable delivery scope — that scoping conversation happens later. Reframed reactions:

**This is a full nonprofit governance + CRM + civic-engagement platform.** The brain-dump spans governance/compliance, institutional knowledge, CRM, community organizing, campaign ops, and public-facing engagement. Rather than narrowing now, the goal is to map the *entire* territory so later scoping decisions are made against a complete picture, not an accidentally-truncated one.

**CRM is a deep, standalone domain** (households/minors, skills/availability, subscriptions, journeys, power-mapping) — comparable in depth to dedicated nonprofit CRMs (Salsa, EveryAction, CiviCRM, NationBuilder). Rather than treating this as a risk to trim, treat it as its own long-range roadmap arc worth fleshing out fully (see expanded list below).

**AI transcription is one instance of a broader "AI-assisted knowledge capture" theme** — worth exploring more broadly: agenda drafting assistance, action-item extraction, decision-summary generation, cross-meeting continuity ("what did we decide about X last quarter"), and conflict/duplicate detection across the document base — not just live transcription. Concrete finding: Notion now has native **AI meeting notes** (`meeting_notes` block) exposed via API with `summary_block_id`, `notes_block_id`, and `transcript_block_id` children — a real candidate to satisfy the transcription requirement without a third-party tool, worth weighing against Otter/Fireflies-style pipelines later.

**Governance and CRM still likely want different data-placement instincts** long-term (human-facing narrative vs. structured/sensitive records), but that's an architecture decision for later, not a reason to omit problem-space territory now.

## Comprehensive problem-space catalog

Organized as long-range roadmap themes — exploratory/beta scoping happens later. Themes beyond the original brain-dump are marked **(new)**.

### Governance & democratic process
- Mandate/charter authoring and amendment workflow, with version history and approval trail.
- Meeting minutes with official votes/decisions; motions, seconds, amendments, tabling.
- Progress reports and roadmap publishing to members and supervising bodies.
- SWOT and other strategic-planning exercises (mission/vision/values, roadmaps).
- **(new)** Election/nomination cycles for officer roles; term limits and rotation schedules.
- **(new)** Voting mechanisms beyond simple motions: proxy voting, ranked-choice, quorum tracking, absentee ballots.
- **(new)** Conflict-of-interest declarations and recusal tracking tied to specific votes.
- **(new)** Bylaws compliance tracking (does a proposed action conform to the charter/bylaws?).
- **(new)** Grievance/dispute resolution process and record-keeping.
- **(new)** Multi-committee/federated hierarchy: parent committee, sub-committees, delegated authority, roll-up reporting.
- **(new)** Public records / freedom-of-information style request handling for transparency-obligated bodies.
- **(new)** Archival & retention policy for governance records (how long minutes/votes are kept, legal hold).

### Meetings & institutional knowledge
- AI meeting transcription (hard requirement) feeding structured minutes.
- Document database for institutional knowledge continuity across volunteer turnover.
- **(new)** Agenda-builder with recurring-item templates (Robert's Rules-style structures).
- **(new)** Action-item extraction and tracking back to owners/deadlines, linked to originating minute.
- **(new)** Cross-meeting knowledge continuity/search ("what did we decide about X").
- **(new)** Succession/handoff packets — auto-assembled knowledge summary when an officer role changes hands.
- **(new)** Meeting logistics: quorum calculation, attendance tracking, scheduling/rescheduling with availability polling.

### CRM & relationship management
- Cultivate consultants and volunteer pool; record interactions; model organizations; power-mapping.
- Subscription and contact preference management.
- Onboarding and other journey tracking.
- Household and minor tracking.
- Interests, capabilities/skills, and availability tracking.
- **(new)** Volunteer hour tracking (impact reporting, insurance/liability requirements, recognition programs).
- **(new)** Skills-based matching (volunteer capability ↔ open committee need).
- **(new)** Membership management: dues, renewals, membership tiers/status.
- **(new)** Donor/fundraising relationship tracking, if the committee fundraises.
- **(new)** Partner-organization and MOU/partnership-agreement tracking.
- **(new)** Consent and data-preference audit trail (who agreed to what, when — especially for minors).

### Community organizing
- Power mapping and organizing best practices.
- **(new)** Stakeholder/coalition mapping beyond power-mapping (allies, opposition, neutral parties, influence networks).
- **(new)** Campaign/issue tracking linked to organizing targets (which ask, which target, which tactic, outcome).
- **(new)** Base-building metrics (growth of engaged supporter base over time).

### Outreach, campaigns & communications
- Outreach/engagement campaign planning; social media and email drafting/scheduling.
- **(new)** Multi-channel campaign calendars (email + social + SMS coordinated sends).
- **(new)** Message testing/feedback loop (which messages land, sentiment tracking on responses).
- **(new)** Press/media kit and crisis-communication plan repository.
- **(new)** Newsletter management (distinct from ad hoc email campaigns — recurring publication workflow).
- **(new)** Multilingual/translation workflow for outreach materials.

### Volunteer onboarding & lifecycle
- Automated onboarding: welcome email, document links, prerequisites (code of conduct, profile completion).
- **(new)** Training/certification tracking (required trainings completed, expirations/renewals).
- **(new)** Offboarding workflow (access revocation, knowledge handoff, exit feedback).
- **(new)** Recognition/gamification (milestones, service awards, ambassador/champion tiers).

### Events & calendars
- Draft/manage/publish event calendars across platforms (Facebook, calendar services, RSS).
- Event registration and sign-up.
- **(new)** Public town-hall/community-meeting support (distinct from internal committee meetings): public notice requirements, public comment capture.
- **(new)** Accommodation requests (accessibility, language interpretation) tied to event registration.
- **(new)** Post-event follow-up automation (thank-you, feedback survey, action-item routing).

### Communications asset library
- Photos and links to photo/video services (YouTube, Vimeo, Flickr); taxonomy; license/attribution tracking.
- **(new)** Rights/consent tracking for photos of individuals (especially minors) distinct from general media licensing.
- **(new)** Brand/style-guide asset repository (logos, templates) alongside media assets.

### Public engagement tooling
- Surveys and public forms; event registration/sign-up.
- **(new)** Public feedback triage and closed-loop response (feedback received → reviewed → responded/incorporated, visible to submitter).
- **(new)** Sentiment/theme analysis across open-ended survey responses.

### Meta-framework: recipe library & federated instances (new)
- Reframing the exploratory goal: the deliverable may not be a single app but a **knowledge-base + solution-recipe library** - a catalog of implementable capabilities (drawn from the themes above) that each org/committee selects from and configures with its own details, rather than one monolithic system everyone runs identically.
- **Federated instance model**: one instance **per committee**, not one per parent organization - a parent organization with several committees would have several independent instances, not a shared database partitioned by committee.
- This raises a sharing/integration challenge distinct from anything above: how a parent org rolls up reporting across its committees' independent instances, and how an individual committee instance stays current as the shared recipe library evolves (new recipes, updated best practice) after its initial setup.
- **Consumption model**: the recipe library is used by both humans (browsing/editing in Notion, the normal way) and coding agents (fetching a recipe's content programmatically while working in a committee's code/config workspace). A recipe is read once by an agent, implemented/applied into the specific instance, and the instance then stands on its own; it does not retain a live link back to the source.
- **Correction (superseded below): recipes live in Notion, not a separate git corpus.** The earlier reasoning that agent-consumability implied a git/markdown corpus assumed Notion pages weren't cleanly machine-readable. That assumption was wrong — see findings below. Notion can be both the human-editable home for the recipe library *and* the agent-fetchable source, so there's no need to split the library across two systems.
- Structurally, this "apply once, then diverge" relationship is close to mka-bootstrap's own `main`/`install` bootstrap pattern (template repo -> `./install.sh` -> independent solution repo) - but notably *without* mka-bootstrap's optional `project-bootstrap` remote for pulling later template updates. Here, a recipe is consumed once and always referenced back to canonical knowledge by citation/link, not by a live update channel. Worth deciding later whether a "recipe was updated, should this instance reapply it" notion is wanted at all, or intentionally out of scope.

### Notion as a publishing platform & markdown source - findings
- **Notion Sites**: any page can be published as a public website with one click (`Share -> Publish`); supports a custom `notion.site` slug (paid plans get custom domains), optional search-engine indexing, and embeddable pages for non-Notion sites.
- **Notion has a first-class markdown API surface** ("enhanced markdown"), separate from the block API: `GET /v1/pages/:page_id/markdown` (read a page as markdown), `POST /v1/pages` with a `markdown` body param (create a page from markdown), and `PATCH /v1/pages/:page_id/markdown` (update via search-and-replace or full replace). This directly satisfies the dual-audience requirement: humans read/edit recipe pages natively in Notion, and an agent can `GET` the same page's markdown to consume a recipe — no separate git corpus needed.
- Most block types map cleanly to markdown (headings, lists, to-dos, quotes, tables, code, images, toggles, callouts, etc.). A few types (bookmarks, embeds, link previews, breadcrumbs) render as `<unknown>` placeholders rather than full markdown — worth avoiding in recipe pages meant for agent consumption, or being aware the agent will need the block API as a fallback for those specific elements.
- Large pages (~20,000+ blocks) can be truncated in a single markdown fetch; the API returns `unknown_block_ids` to page through recursively. Not a near-term concern at recipe-library scale, but notable for very long pages.
- **"Duplicate as template"** is a separate, per-site toggle that lets a visitor copy a published page into their own workspace as a live Notion page/database. Distinct from the markdown API above — this is a human-facing "clone this into my workspace" action (e.g., handing a new committee's admin a starter page), not how an agent would consume a recipe.
- **Architecture constraint worth keeping regardless**: Notion integrations/API access are scoped per-workspace - there is no live cross-workspace query. Any given committee's Notion workspace is architecturally isolated from others and from a shared library; each instance's automation/integration credentials are its own. This means an agent working in a committee's own workspace needs its *own* connection/credentials with read access to the shared recipe-library workspace (or the library needs to be published such that a personal-access-token/public connection can read it) — a cross-workspace read path still needs to be designed, even though it's a read of markdown rather than a page duplication.
- Notion also supports selling templates via Marketplace directly from a published site - not a current goal, but notable if a human-facing template is ever wanted in addition to the agent-fetchable recipe pages.

### Cross-cutting / platform-level themes (new)
- **Roles & permissions**: volunteer vs. officer vs. public-visitor access tiers across every module above.
- **Accessibility & compliance**: WCAG conformance for public-facing artifacts, ADA accommodation workflow.
- **Financial/budget/treasury tracking**: budget vs. actuals reporting alongside progress reports; grant-funded committees tracking grant compliance.
- **Notifications**: cross-channel reminders (email/SMS/push) for tasks, meetings, renewals.
- **Audit trail & security**: who changed what governance record, when — relevant given official votes/decisions are legally/organizationally consequential.
- **Data portability/export**: since target orgs are resource-constrained and volunteer-run, an exit path (export everything) matters for long-term trust.
- **Offline/low-bandwidth access**: some volunteer populations may have limited connectivity or device access.
- **Multi-tenancy**: supporting many independent committees/organizations on shared infrastructure vs. one committee per deployment.

## Open questions to resolve before drafting the Manifest

_(Scope/prioritization questions deferred to a later agenda item — not resolved here.)_

1. Which of the above themes are core to the committee-governance product vision vs. adjacent/future?
2. Which supervising-body reporting formats are real vs. hypothetical (need real-world examples)?
3. What's the concrete AI-transcription tool/pipeline choice, and does the broader "AI-assisted knowledge capture" theme extend further (agenda drafting, decision summarization, cross-meeting search)?
4. How deep does CRM need to go (households/minors/skills/availability) as a long-range target, independent of near-term build scope?
5. Is multi-tenancy (many committees on shared infrastructure) part of the long-range vision, or is this always single-committee-per-deployment?
