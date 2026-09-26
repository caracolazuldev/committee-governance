# Notion API capability findings (M-014)

Working note for M-014 (validate Notion API capabilities), tested against a dedicated test page shared with the integration (no other workspace content touched). All calls used `Notion-Version: 2022-06-28`.

## Access model

- Integration tokens have **zero page access by default**. A human must explicitly share at least one page/database with the integration in the Notion UI before any API call can see it — this cannot be granted via API. Confirmed by `POST /v1/search` returning only the one page a human had shared.

## Content retrieval (markdown behavior)

- There is **no markdown in/out** on the public API. Content is exchanged as structured block JSON (`GET/PATCH /v1/blocks/{id}/children`), not markdown text.
- Markdown syntax typed into `rich_text.text.content` (e.g. `**bold**`) is stored and returned **literally as plain text** — Notion does not auto-parse it. Formatting must be set explicitly via each rich-text span's `annotations` object (`bold`, `italic`, etc.), not via markdown syntax.
- Any Docs-to-Notion bridge (M-017) will need an explicit markdown → block-JSON converter; there is no endpoint that accepts raw markdown.

## Page/content creation and update

- `PATCH /v1/blocks/{id}/children` (append) and `PATCH /v1/blocks/{block_id}` (update single block) both worked as expected (HTTP 200) for headings, paragraphs, bulleted list items, and code blocks.
- `POST /v1/pages` successfully created a child page under the test page.

## Public-read behavior

- A page's `public_url` field is **read-only via the public API**. Attempting `PATCH /v1/pages/{id}` with `public_url` set returns **HTTP 200 but silently ignores the field** — no error, no change. Publishing a page to the web (Notion Sites) is a UI-only action; it cannot be automated via API.
- The test page's `public_url` was `None` throughout (never published), confirming authenticated-only access was exercised; public-read behavior for a *published* page would need to be tested separately by a human publishing a page first.

## Schema operations (recipe-provisioning relevance)

- `POST /v1/databases` successfully created a database with `title`, `select`, `multi_select`, `people`, `checkbox`, and `rich_text` properties in one call.
- `PATCH /v1/databases/{id}` successfully added a property (`number`) and removed one (`rich_text`) in the same request (`null` removes a property).
- A **formula property was successfully added via schema patch** (`{"formula": {"expression": "1 + 1"}}`, HTTP 200) — contrary to an initial assumption that formula properties might be API-restricted. No schema operation tested so far was rejected.

## Rate limits

- A burst of 30 sequential authenticated `GET` requests completed in ~12.5s (~2.4 req/s average) without any `429`. Consistent with Notion's documented average limit of ~3 requests/second; no throttling response was intentionally forced (avoided hammering the API further than needed for this smoke test).

## Cleanup

- The test child page and test database created during this spike were archived (soft-deleted) via `PATCH .../archived: true`. The original shared test page ("ExpImp API Test") was left in place as the reusable test fixture.

## Open items for later milestones

- Confirming actual public-read behavior of a *published* page (needs a human to publish once, then an unauthenticated fetch).
- Markdown → block-JSON conversion approach for M-017 (Docs-to-Notion bridge).
