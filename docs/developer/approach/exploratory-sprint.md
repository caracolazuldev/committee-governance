# Exploratory implementation sprint

## Purpose

Exercise the smallest useful slice of the proposed platform so observed API behavior, access constraints, costs, and failure modes can determine the final MVP scope and implementation requirements. This sprint is discovery through working prototypes, not delivery of production-ready committee workflows.

## Scope and guardrails

- Set up access for Notion, Zapier, and Google Docs/Sheets. Store secret values only in approved secret storage or local environment configuration; never commit them or include them in logs, screenshots, or this documentation.
- Use dedicated test pages, documents, and automation data. Do not modify a production committee workspace.
- Use Zapier's API, not its no-code UI builder, for the automation experiment.
- Facebook, including Facebook credentials, Graph API calls, posts, and Events, is explicitly outside this initial sprint.
- Airtable and EmailOctopus are also outside the initial integration slice. Their MVP candidacy may be reviewed during closeout, but no credentials or integrations are required in this sprint.
- Record observed account plans, permissions, rate limits, API behavior, manual steps, and costs without recording secret values.

## Milestones

### M-013 Set up exploratory integration credentials

Provision credentials for Notion, Zapier, and Google Docs/Sheets with the minimum access needed for the test activities. Confirm where credentials are stored and which scopes or sharing grants are required. Google authorization is completed via `gcloud` Application Default Credentials on the host machine — see [google-adc-setup.md](../google-adc-setup.md) for the confirmed command and rationale.

**Depends on:** None.

**Done when:** A harmless authenticated smoke check succeeds for each service; the required scopes, account/plan constraints, and secret-storage location are documented; and a repository scan confirms no credential values were added.

**Evidence:**

- **Storage:** All secrets live under `~/.config/cmte-gvrnce/` on the host (bind-mounted into the devcontainer), file mode `600`, gitignored. Repo scan for common credential patterns and for the credential filenames themselves returned no matches in tracked or staged content.
- **Notion:** Bearer token in `NOTION_API_TOKEN`. Smoke check: `GET /v1/users/me` → HTTP 200.
- **Zapier:** Deploy key in `ZAPIER_DEPLOY_KEY` (read by the Zapier CLI directly). Smoke check: `integrations --format=json` → authenticated, 0 integrations (valid empty result, not an auth failure).
- **Google Docs/Sheets:** OAuth via `gcloud auth application-default login` with a project-owned client (Desktop type) and explicit scopes (`cloud-platform` required by `gcloud`, plus `documents`, `spreadsheets`, `userinfo.email`, `openid`). Smoke check: refreshed token verified via `tokeninfo` (correct audience, correct scopes) and harmless `GET` probes against the Sheets and Docs APIs both returned HTTP 404 (endpoint reached and token accepted; resource simply doesn't exist) rather than 401/403.

### M-014 Validate Notion API capabilities

Use a dedicated test page to exercise markdown retrieval and page creation or update. Check public-read behavior separately from authenticated access and probe only the schema operations needed to assess recipe provisioning. Findings: [notion-api-findings.md](../notion-api-findings.md).

**Depends on:** M-013.

**Done when:** A repeatable test records the request/response behavior, permission requirements, markdown limitations, schema operation results, and relevant rate limits. The test leaves no production workspace changes.

### M-015 Validate Google Docs and Sheets access

Create and read a controlled Google Docs draft and spreadsheet. Determine which identifiers and metadata support the proposed tag-and-file convention and recruitment form destination, and whether needed content can be read and written in a usable form. Findings: [google-docs-sheets-findings.md](../google-docs-sheets-findings.md).

**Depends on:** M-013.

**Done when:** A reproducible test document and spreadsheet are created and retrieved; required API scopes and sharing settings are recorded; and metadata, export, and manual-step constraints relevant to the bridge and form destination are documented.

### M-016 Validate Zapier API-driven automation

Use a harmless test trigger and sink to determine whether the Zapier API supports the required automation lifecycle: configuration, activation or test execution, run inspection, and retry/error visibility. Findings: [zapier-automation-findings.md](../zapier-automation-findings.md).

**Depends on:** M-013.

**Done when:** The test automation completes through API-driven setup and execution; its run result can be inspected; retry and failure behavior are exercised or explicitly identified as unavailable; and plan, polling, and API constraints are recorded.

### M-017 Demonstrate a Docs-to-Notion tag-and-file slice

Exercise the proposed collaboration bridge with a tagged test draft. Use the M-016 automation pattern where supported; identify any steps that require a human or a different integration mechanism.

**Depends on:** M-014, M-015, and M-016.

**Done when:** The test draft produces a corresponding Notion page or a documented, reproducible blocker; the resulting page links back to its source; and duplicate, retry, and failure behavior is recorded. No Facebook integration is attempted.

### M-018 Explore meeting facilitation UX in Notion

Prototype a meeting workspace in a test Notion instance that guides users through agenda creation, meeting conduct, human note-taking, and making, amending, debating, and disposing of motions. Explore how guidance appears at the moment of use without overwhelming the working meeting record. Findings: [meeting-facilitation-findings.md](../meeting-facilitation-findings.md).

Treat procedural guidance as configurable support, not authoritative rules: committees may follow different bylaws, standing rules, or consensus practices. Walk through a representative meeting scenario with a facilitator or participant and record confusion, missing steps, and facilitation needs.

**Depends on:** M-013 and M-014.

**Done when:** The test workspace contains a linked agenda and meeting-record prototype covering the named activities; a scenario walkthrough is recorded; and usability findings, configurable guidance needs, and unresolved policy questions are documented.

### M-019 Explore meeting and collaboration document library UX

Prototype a Notion library experience for meeting documents, shared resources, and collaboration documents. Explore how people browse, classify, search, relate, and reuse documents without confusing public records with internal working material. Findings: [document-library-findings.md](../document-library-findings.md).

**Depends on:** M-013, M-014, and M-015.

**Done when:** The test workspace contains representative meeting, resource, and collaboration documents; a proposed navigation/taxonomy, metadata set, views, and meeting links are documented; and a walkthrough verifies retrieval and access boundaries for internal versus public content.

### M-020 Prototype recruitment form to Google Sheets

Create a test recruitment form and process each submission through Zapier into a restricted Google Sheet. Explore the respondent experience, minimum useful fields, consent language, and maintainer follow-up workflow. Use synthetic test data only.

**Depends on:** M-013, M-015, and M-016.

**Done when:** A test submission creates a correctly mapped spreadsheet row; form respondents cannot view other submissions; access to the sheet is restricted to authorized maintainers; and duplicate, retry, and failure behavior is tested or documented as unsupported.

### M-021 Reconcile discovery into MVP requirements

Turn sprint evidence into a recommendation for final MVP scope and implementation requirements. Separate validated capabilities from assumptions and unresolved decisions.

**Depends on:** M-013 through M-020.

**Done when:** The closeout records evidence and constraints for each tested integration and UX prototype, required permissions and account plans, operational and security requirements, known failure modes, open decisions, and a recommended MVP scope/sequence. Update the candidate [MVP roadmap](mvp-roadmap.md) and [MVP approach](mvp-approach.md) to reflect the findings.

## Sprint exit

The sprint is complete when M-021 is accepted. Its output is a decision-ready scope and implementation-requirements record, not a production deployment. Features not exercised or documented as evidence-backed decisions remain unvalidated candidates.