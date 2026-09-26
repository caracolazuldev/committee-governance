# Zapier API-driven automation findings (M-016)

Working note for M-016. Tested exclusively through `zapier-platform` CLI `19.1.0`; the no-code Zap Editor was not used.

## What the CLI/API supports (confirmed)

1. Created and registered a private, throwaway integration from the CLI.
2. Built and uploaded versions `1.0.0` and `1.0.1` with `zapier-platform push`.
3. Executed a polling trigger and a harmless create action remotely with `zapier-platform invoke --remote`. The trigger used the public JSONPlaceholder API; the action posted harmless test data to the same service. Both returned successful results.
4. Executed an intentional remote failure. The CLI returned the runtime error and stack trace; `zapier-platform logs --format=json` returned the same error record for version `1.0.1`.
5. Deleted the temporary private integration and both test versions with `zapier-platform delete:app`.

## Automation-lifecycle boundary (confirmed)

The CLI is an API for **custom integration development**, not for individual user Zaps. It supports schema/code registration, version upload, and direct operation invocation, but it has no command to create or activate a trigger-to-action Zap, poll a Zap task/run, or retry a failed Zap task. The CLI's `history` command is integration-change history, not task history.

Consequences for the required lifecycle:

| Required capability | Result |
|---|---|
| Configure integration | Confirmed through CLI registration and push |
| Execute integration logic | Confirmed through remote `invoke` |
| Inspect execution/failure | Confirmed through CLI result and integration logs |
| Poll actual Zap task history | Unavailable through this CLI workflow |
| Retry actual Zap task | Unavailable through this CLI workflow |
| Configure/activate a user Zap without the no-code UI | Not established; no CLI command supports it |

## Plan, polling, and API constraints

- A deploy key permits integration development operations; it does not expose a general API for user Zap lifecycle management.
- `invoke --remote` runs the selected operation on Zapier production infrastructure, but is not a persisted/activated automation and does not produce an end-user Zap task to retry.
- Remote integration logs retain operation failures; local invocations do not reach Zapier and therefore do not appear in those logs.
- The test account had no pre-existing integrations before the spike. No plan-specific task quota or polling interval was exercised because a runnable user Zap could not be configured through the permitted API/CLI surface.

## M-017 implication

The Docs-to-Notion slice cannot assume Zapier CLI can provision or run a user automation. It must either identify a supported API surface outside the Platform CLI or record a human Zap-configuration step as a reproducible manual boundary.
