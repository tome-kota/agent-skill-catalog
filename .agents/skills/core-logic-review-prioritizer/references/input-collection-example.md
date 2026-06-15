# Input Collection Example

Use a lightweight collection pattern before reviewing.

## Example

`review artifact contract`

- `diff`: PR #482 against `main`
- `base`: `origin/main`
- `spec sources`: PR description, `docs/billing-renewal.md`, ADR-019, linked issue `BILL-241`
- `exclusions`: `generated/`, snapshots, dependency lockfile refresh
- `known gaps`: no downstream consumer list for the new billing event

## Minimal Command Pattern

Adapt to the repo, but a typical collection flow looks like:

- identify the diff range or PR number
- identify the base branch
- identify design-intent sources such as PR text, ADRs, or issues
- exclude generated or mechanical-change areas before hotspot selection

## Bad Collection Pattern

Avoid starting with only:

- a raw diff with no base reference
- no design-intent source
- generated files mixed into hotspot selection
- no explicit record of missing information
