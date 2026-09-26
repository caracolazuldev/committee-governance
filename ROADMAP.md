# ── Completed ──
# ── Acceptance ──

## M-013 Set up exploratory integration credentials

Provision and verify credentials for Notion, Zapier, and Google Docs/Sheets using approved secret storage. Record required scopes and setup steps without committing secret values. See the [exploratory implementation sprint](docs/developer/approach/exploratory-sprint.md).

## M-014 Validate Notion API capabilities

Run a controlled read/write and public-read spike against a test page; record permissions, markdown behavior, schema operations, limits, and any unsupported needs. No production workspace changes.

## M-015 Validate Google Docs and Sheets access

Verify programmatic access to create/read a controlled draft and spreadsheet. Record account, sharing, export, and API constraints relevant to the document bridge and recruitment intake.

## M-016 Validate Zapier API-driven automation

Prove that a small test automation can be configured, run, inspected, and retried through the API, using a harmless test action. Record trigger, polling, task-history, and failure-handling constraints; do not use the no-code UI builder.

## M-019 Explore meeting and collaboration document library UX

Prototype a Notion library for meeting documents, shared resources, and collaboration documents. Test navigation, classification, metadata, search/filtering, links from meetings, and internal versus public access.

# ── Quality Assurance ──
# ── In Progress ──
# ── Sprint ──

## M-017 Demonstrate a Docs-to-Notion tag-and-file slice

Use a tagged test draft to exercise the narrow Google Docs-to-Notion knowledge-base flow, using Zapier where supported. Verify the resulting page and source traceability, and document manual steps or failure modes. Facebook is explicitly outside this sprint.

## M-018 Explore meeting facilitation UX in Notion

Prototype agenda creation, meeting conduct guidance, human note-taking, and motion creation, amendment, debate, and disposition in a test Notion workspace. Validate the flow with a scenario walkthrough and record usability findings; guidance must be configurable to committee rules and practices.

## M-020 Prototype recruitment form to Google Sheets

Create a test recruitment form and process submissions through Zapier into a restricted Google Sheet. Verify field mapping, submission privacy, duplicate/retry behavior, and failure visibility using test data only.

## M-021 Reconcile discovery into MVP requirements

Publish evidence-based implementation requirements, open decisions, and a recommended MVP scope and sequence from M-013 through M-020. Update the candidate [MVP roadmap](docs/developer/approach/mvp-roadmap.md) and [MVP approach](docs/developer/approach/mvp-approach.md); do not treat unvalidated features as committed scope.

# ── Scoping ──
