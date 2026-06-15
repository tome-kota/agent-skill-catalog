# Handoff Examples

Use these examples to keep the final handoff concrete and compact.

## Good Handoff

`review target selection`

- `review deeply`
  - `renewal_state_machine.ts`
  - `priority reason`: state-transition meaning changed and failure impact is high
- `review if needed`
  - `billing_retry_worker.ts`
  - `priority reason`: retry placement may have shifted, but evidence of semantic change is limited
- `safe to skim`
  - UI copy updates in billing settings
- `delegate to automation`
  - regenerated GraphQL types

`extracted design concerns`

- the new `past_due` transition may shift permission and retry behavior
- retry ownership may now be split between the worker and the service

`agent view`

- the diff suggests clearer state naming, but decision ownership around retry still looks unstable

`recommended reviewer action`

- compare `past_due` behavior against the billing ADR
- confirm with the domain owner whether retry is supposed to remain worker-owned

Why this is good:

- attention is clearly limited
- every deep item has a reason
- the reviewer gets concrete next steps

## Weak Handoff

`review target selection`

- many files changed in billing
- controller, service, worker, tests, and events all touched

`agent view`

- this looks complex and risky

`recommended reviewer action`

- review carefully

Why this is weak:

- no prioritization
- no evidence-backed concern
- no reviewer question
- no actionable next step
