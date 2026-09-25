# Exploratory implementation sprint

## Purpose

Exercise the smallest useful slice of the proposed platform so observed API behavior, access constraints, costs, and failure modes can determine the final MVP scope and implementation requirements. This sprint is discovery through working prototypes, not delivery of production-ready committee workflows.

## Scope and guardrails

- Set up access for Notion, Zapier, and Google Docs. Store secret values only in approved secret storage or local environment configuration; never commit them or include them in logs, screenshots, or this documentation.
- Use dedicated test pages, documents, and automation data. Do not modify a production committee workspace.
- Use Zapier's API, not its no-code UI builder, for the automation experiment.
- Facebook, including Facebook credentials, Graph API calls, posts, and Events, is explicitly outside this initial sprint.
- Airtable and EmailOctopus are also outside the initial integration slice. Their MVP candidacy may be reviewed during closeout, but no credentials or integrations are required in this sprint.
- Record observed account plans, permissions, rate limits, API behavior, manual steps, and costs without recording secret values.

## Milestones

### M-013 Set up exploratory integration credentials

Provision credentials for Notion, Zapier, and Google Docs with the minimum access needed for the test activities. Confirm where credentials are stored and which scopes or sharing grants are required.

**Depends on:** None.

**Done when:** A harmless authenticated smoke check succeeds for each service; the required scopes, account/plan constraints, and secret-storage location are documented; and a repository scan confirms no credential values were added.

### M-014 Validate Notion API capabilities

Use a dedicated test page to exercise markdown retrieval and page creation or update. Check public-read behavior separately from authenticated access and probe only the schema operations needed to assess recipe provisioning.

**Depends on:** M-013.

**Done when:** A repeatable test records the request/response behavior, permission requirements, markdown limitations, schema operation results, and relevant rate limits. The test leaves no production workspace changes.

### M-015 Validate Google Docs collaboration access

Create and read a controlled draft using the available Google Docs access. Determine which identifiers and metadata can support the proposed tag-and-file convention and whether the needed content can be read in a usable form.

**Depends on:** M-013.

**Done when:** A reproducible test document is created and retrieved; required API scopes and sharing settings are recorded; and metadata, export, and manual-step constraints relevant to the bridge are documented.

### M-016 Validate Zapier API-driven automation

Use a harmless test trigger and sink to determine whether the Zapier API supports the required automation lifecycle: configuration, activation or test execution, run inspection, and retry/error visibility.

**Depends on:** M-013.

**Done when:** The test automation completes through API-driven setup and execution; its run result can be inspected; retry and failure behavior are exercised or explicitly identified as unavailable; and plan, polling, and API constraints are recorded.

### M-017 Demonstrate a Docs-to-Notion tag-and-file slice

Exercise the proposed collaboration bridge with a tagged test draft. Use the M-016 automation pattern where supported; identify any steps that require a human or a different integration mechanism.

**Depends on:** M-014, M-015, and M-016.

**Done when:** The test draft produces a corresponding Notion page or a documented, reproducible blocker; the resulting page links back to its source; and duplicate, retry, and failure behavior is recorded. No Facebook integration is attempted.

### M-018 Reconcile discovery into MVP requirements

Turn sprint evidence into a recommendation for final MVP scope and implementation requirements. Separate validated capabilities from assumptions and unresolved decisions.

**Depends on:** M-013 through M-017.

**Done when:** The closeout records evidence and constraints for each tested integration, required permissions and account plans, operational and security requirements, known failure modes, open decisions, and a recommended MVP scope/sequence. Update the candidate [MVP roadmap](mvp-roadmap.md) and [MVP approach](mvp-approach.md) to reflect the findings.

## Sprint exit

The sprint is complete when M-018 is accepted. Its output is a decision-ready scope and implementation-requirements record, not a production deployment. Features not exercised or documented as evidence-backed decisions remain unvalidated candidates.