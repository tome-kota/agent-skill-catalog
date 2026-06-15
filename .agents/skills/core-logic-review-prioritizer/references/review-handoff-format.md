# Review Handoff Format

The handoff should be short, ranked, and immediately usable by a human reviewer.

## Base Template

`review artifact contract`

- `diff`
- `base`
- `spec sources`
- `exclusions`
- `known gaps`

`review target selection`

- `review deeply`
- `review if needed`
- `safe to skim`
- `delegate to automation`
- why those priorities were chosen
- each hotspot’s `priority reason`

`extracted design concerns`

- which responsibility placements are under question
- which business-rule, control-decision, state-transition, or contract-boundary expressions are under question
- which workaround smells are under question

`agent view`

- the skill’s concerns or hypotheses
- written as review support, not as a final verdict

`recommended reviewer action`

- what the human should verify next
- who may need to be consulted
- whether the likely next step is accept, hold, or follow-up cleanup

`optional visualization`

- only when useful
- explain which judgment it helps accelerate

## Count Limits

- `review deeply`: at most 3 items
- `review if needed`: at most 5 items
- `safe to skim`: grouping is allowed
- `delegate to automation`: grouping is allowed

If the list would exceed these limits, group instead of enumerating everything.

## Minimum Fields Per Hotspot

- `target`: what to inspect
- `location`: file, function, or responsibility unit
- `evidence`: which concrete diff fact supports the concern
- `concern`: what the design concern is
- `priority reason`: why it belongs in this review bucket
- `view`: the skill’s concern or hypothesis
- `question`: what the human should verify
- `recommended action`: what to do next

`evidence` must include at least one concrete diff fact.

- a concrete changed condition, state, branch, field, retry rule, permission edge, payload change, or similar diff fact

Do not use a file name or function name alone as evidence. Those belong in `location`.

## When To Use Visualization

Visualization is a support tool. In most cases, a ranked table or concise handoff is enough.

Use it when:

- exploration cost is high
- multiple reviewers need to coordinate
- structural drift or rule drift is easier to understand at a glance

### Structure Drift View

Recommended template:

| location | drift signal | reviewer question | action |
| --- | --- | --- | --- |
| module / layer | responsibility drift or dependency reversal | what to verify | inspect / hold / consult |

### Rule Fidelity View

Recommended template:

| rule or transition | changed fact | reviewer question | action |
| --- | --- | --- | --- |
| rule / state | concrete diff fact | what to verify | inspect / compare / consult |

### Direction View

Recommended template:

| location | direction | concern | follow-up |
| --- | --- | --- | --- |
| hotspot | improved / neutral / regressed | workaround hardening or similar concern | whether follow-up cleanup is worth proposing |

## Writing Guidance

- prefer short ranked bullets over long summaries
- give business, structural, or control meaning where possible
- state uncertainty when it exists
- make the next action obvious
- avoid claims not supported by `evidence`
