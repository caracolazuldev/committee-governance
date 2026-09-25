# ── Completed ──
# ── Acceptance ──
# ── Quality Assurance ──
# ── In Progress ──
# ── Sprint ──

## M-013 Set up exploratory integration credentials

Provision and verify credentials for Notion, Zapier, and Google Docs using approved secret storage. Record required scopes and setup steps without committing secret values. See the [exploratory implementation sprint](docs/developer/approach/exploratory-sprint.md).

## M-014 Validate Notion API capabilities

Run a controlled read/write and public-read spike against a test page; record permissions, markdown behavior, schema operations, limits, and any unsupported needs. No production workspace changes.

## M-015 Validate Google Docs collaboration access

Verify programmatic access to create and read a controlled draft and capture the metadata available for the proposed tag-and-file workflow. Record account, sharing, export, and API constraints.

## M-016 Validate Zapier API-driven automation

Prove that a small test automation can be configured, run, inspected, and retried through the API, using a harmless test action. Record trigger, polling, task-history, and failure-handling constraints; do not use the no-code UI builder.

## M-017 Demonstrate a Docs-to-Notion tag-and-file slice

Use a tagged test draft to exercise the narrow Google Docs-to-Notion knowledge-base flow, using Zapier where supported. Verify the resulting page and source traceability, and document manual steps or failure modes. Facebook is explicitly outside this sprint.

## M-018 Reconcile discovery into MVP requirements

Publish evidence-based implementation requirements, open decisions, and a recommended MVP scope and sequence from M-013 through M-017. Update the candidate [MVP roadmap](docs/developer/approach/mvp-roadmap.md) and [MVP approach](docs/developer/approach/mvp-approach.md); do not treat unvalidated features as committed scope.

# ── Scoping ──
