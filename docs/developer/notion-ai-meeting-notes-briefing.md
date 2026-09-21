# Briefing: Notion AI Meeting Notes

Reference briefing to shelve this topic for later. Consolidates research from the 2026-09-20 architecture brainstorm meeting (see [missives/2026-09-20-architecture-brainstorm-meeting-notes.md](../../missives/2026-09-20-architecture-brainstorm-meeting-notes.md), agenda item 5) so it doesn't need to be re-researched.

## What it is

Notion's native AI transcription + summarization feature for meetings ("AI Meeting Notes"). Produces a transcript, an AI-generated summary, and structured notes/action items, attached to a page as a `meeting_notes` block.

## How it can be triggered

1. **Live, in the Notion desktop/mobile app** — type `/meet`, click `Start transcribing` during a call. Captures system audio (desktop) or mic audio (mobile/browser). Speaker labeling works best for 1:1 virtual calls; less reliable for group/in-person meetings or shared microphones.
2. **Upload an existing recording** — attach an audio file (AAC, M4A, MP3, WAV; not MOV/MP4/Loom) to a meeting notes block via the UI.
3. **Via the public API** — `POST /v1/blocks/meeting_notes`, with a completed file upload as the source and a target page as parent. This is the automatable path (usable from Zapier or a script), confirmed via [developers.notion.com/reference/create-meeting-note](https://developers.notion.com/reference/create-meeting-note):
   - Processing is asynchronous — poll the returned block (`GET` a block) until `meeting_notes.status` is `notes_ready`, then read `summary_block_id`, `notes_block_id`, `transcript_block_id`.
   - Requires the integration to have **Insert content** (and **Read content**) capability.
   - Requires the associated user to have AI Meeting Notes entitlement (see cost, below) — the API does **not** bypass the plan requirement.
   - Not idempotent — retries can create duplicate meeting notes blocks.

## Reading the output

- Via markdown API (`GET /v1/pages/:page_id/markdown?include_transcript=true`) — meeting notes render as a `<meeting-notes>` tag; transcript included only with the query param.
- Via block API — fetch `summary_block_id` / `notes_block_id` / `transcript_block_id` children directly.

## Cost — the actual gating factor

Confirmed via [notion.com/pricing](https://www.notion.com/pricing):

- **Meeting notes is a Business-plan feature** ($20/seat/month, billed per paid workspace member). Free/Plus plans only get a "Limited Trial."
- This applies to **the entitlement**, not the recording method — using the API with an externally recorded/uploaded file still requires the same Business-tier access as the live desktop flow. **Recording outside Notion does not avoid this cost.**
- The only way to avoid the Business-plan cost while still landing meeting content in Notion is to **not use this feature at all**: transcribe/summarize elsewhere (free/cheap tool, or a conferencing platform's built-in transcript) and write the resulting plain markdown into a normal page via the standard Public API, which works on every plan including Free.

## Related but distinct: Notion Agents

Notion Agents (the built-in Notion Agent, plus configurable Custom Agents) are a **separate** product feature, not part of AI Meeting Notes itself:

- Run inside Notion, connect natively to **Slack, Mail, Calendar, and MCP integrations**.
- Can be configured (via Notion's own agent-builder UI, natural-language setup) to pick up action items and distribute recaps automatically — this is what "the recap sends itself" / "your agent handles action items" marketing claims actually refer to.
- Separate entitlement/cost from Meeting Notes: Business/Enterprise plan, plus consumption of "Notion credits" for Custom Agents.
- **Unconfirmed**: whether Custom Agents are configurable/triggerable via the public REST API, or UI/agent-builder only. Not yet investigated in depth.

## Legal/consent note

Recording/transcribing meetings has consent requirements that vary by jurisdiction (one-party vs. all-party consent in the US; similar rules in the EU). Notion provides consent-message tooling (text or spoken) but obtaining valid consent is the workspace's/user's responsibility, not something the product guarantees compliance for.

## Open decision (not resolved here)

Two viable architecture paths, deliberately left undecided:

1. **Use Notion's native transcription** — requires Business plan; gets polished transcript/speaker labels/AI summary without building that ourselves.
2. **Bring your own transcription** — any plan; requires sourcing transcription elsewhere and building the condense-into-minutes step ourselves.

Given the target user (resource-constrained volunteer committees), option 2 is the more likely default, with option 1 as a budget-permitting upgrade — to be decided when this topic is picked back up.
