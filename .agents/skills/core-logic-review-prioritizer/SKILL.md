---
name: core-logic-review-prioritizer
description: Use this skill when a large AI-assisted code change needs human review focused on the design validity of business logic, core decision logic, or system-critical control logic. Instead of reading the full diff line by line, the skill organizes the parts a human should inspect deeply, the design concerns involved, the questions that should be answered, and the next actions to take. It is especially useful for changes that affect rule expression, responsibility placement, permission boundaries, contracts, state transitions, control decisions, or areas where ad hoc fixes and workarounds are likely to appear. Final judgment on design validity remains with the human reviewer. Use visualization only when it materially helps review. Do not use this skill for small diffs, style-only review, or changes where full close reading is still practical.
---

# Core Logic Design Review Support

The primary purpose of this skill is to help a human reviewer assess the design validity of business logic, core decision logic, and system-critical control logic in a short amount of time by organizing what to inspect, which design concerns matter, which questions to answer, and what next actions to take.

This is not a general-purpose code review replacement. Its job is to reshape a large change so human attention goes to the places where design judgment matters.

## Responsibilities

This skill is responsible for:

- selecting the review targets a human should inspect deeply
- extracting design concerns from business logic and core decision logic changes
- surfacing the questions a human should explicitly check
- offering review-supporting views and review hypotheses
- suggesting next actions for the reviewer
- using visualization only when it meaningfully improves review

## Non-Responsibilities

This skill is not responsible for:

- making the final judgment on design validity
- approving a change on behalf of a human
- performing a full general-purpose code quality review

## Core Stance

- prioritize narrowing attention over reading everything
- prioritize business meaning and structural validity over surface neatness
- look at changes in responsibilities, boundaries, rule expression, and control decisions rather than line counts
- do not miss ad hoc fixes or workaround-shaped structure
- optimize first for faster, better human judgment
- treat improvement direction and Leave It Better as useful supporting lenses, not the main goal every time

## Main Deliverables

The skill’s primary value is in these four outputs:

- `target selection`: identify where human attention should go first
- `concern extraction`: translate diffs into design-review concerns
- `question framing`: make explicit what the reviewer should verify
- `next-action guidance`: suggest deeper inspection, comparison, consultation, acceptance, or follow-up cleanup

## Supporting Lenses

The following lenses are useful, but they are not the main job of the skill. Use them only when they help the human make a design judgment faster.

- `design fidelity`: whether rules and control logic are expressed in the intended responsibility and boundary structure
- `structural validity`: whether placement, dependency direction, and responsibility split make sense
- `direction of change`: whether the change moved the structure in a better or worse direction
- `workaround risk`: whether the change hardens ad hoc branches, local patches, or responsibility leakage
- `KISS`: whether unnecessary complexity or abstraction was introduced
- `understandability`: whether a human can still locate the key decisions easily
- `changeability`: whether the next similar change will be easier, not harder

Do not use these as a separate generic quality checklist. Use them only as supporting lenses for faster human design review.

## Visualization

Visualization is not for making a diff look nicer. In most cases a short ranked handoff is enough. Use visualization only when:

- review exploration cost is high
- reviewers need to split work
- structural drift or rule drift is easier to communicate at a glance

Use it only when it helps answer where meaning changed, where design judgment matters, where ad hoc handling appeared, or what can safely be skimmed.

## Workflow

Follow this sequence:

1. Gather the review inputs.
2. Decide whether this request is a fit for the skill.
3. Frame what the human reviewer actually needs to judge.
4. Find the business logic, core decision logic, and system-critical control logic hotspots.
5. Translate those hotspots into design-review concerns.
6. Produce review-supporting views and recommended reviewer actions.
7. Add visualization only if it helps materially.
8. Produce the review handoff.

## 1. Gather The Review Inputs

To keep the process reproducible, confirm these inputs first:

- `target diff`: what change is being reviewed
- `base`: what the diff is relative to
- `spec sources`: PR text, design memo, ADR, issue, existing rule documentation, or other intent sources
- `exclusions`: generated files, vendor code, snapshots, or other low-value review areas
- `gap handling`: whether missing information should be carried as an explicit assumption, downgraded confidence, or a weaker application of the skill

When possible, summarize this as a short `review artifact contract`:

- `diff`
- `base`
- `spec sources`
- `exclusions`
- `known gaps`

If inputs are missing, do not pretend to have higher confidence than the evidence supports. Carry the missing information forward explicitly.

For a concrete example, read [references/input-collection-example.md](references/input-collection-example.md).

## 2. Decide Whether This Request Fits The Skill

First determine whether this skill should be foregrounded.

Good fit examples:

- the diff is large enough that reading everything closely is expensive
- the change contains business logic, core decision logic, permission logic, contract boundaries, state transitions, or system-critical control decisions
- ad hoc fixes or workaround-shaped structure may be present
- bad time allocation would likely cause the reviewer to miss the important areas

Poor fit examples:

- the diff is small enough that full reading is practical
- the change is mostly style, lint, mechanical rename, or generated output
- the main subject is a local fix rather than core decision logic

When in doubt, use this simple rule:

- `full application`: the main subject includes business rules, responsibility placement, state transitions, contract change, permission logic, or system-critical control decisions, and target selection is more valuable than close-reading everything
- `light application`: there are design concerns, but the diff is only medium-sized and ordering the review is more important than a full structured handoff
- `not a fit`: design concerns are thin and a normal review or short diff summary is enough

If the request is not a fit, do not force core-logic concerns into it.

Standard output when it is not a fit:

- `not a fit`: why the skill is not a good fit
- `suggested fallback`: normal review, short diff summary, or another skill if appropriate
- `lightweight output`: optionally return only a short summary of the change

## 3. Frame What The Reviewer Needs To Judge

Define the design-review focus briefly. Do not try to conclude correctness here; align the review focus instead.

- which business rules, core decision logic, or system-critical control decisions changed
- which responsibility placements or rule expressions look likely to matter
- whether the central question is correctness, responsibility placement, or workaround-shaped handling

Assume the reviewer does not want a prettier diff. They want the shortest path to the design judgments that matter.

## 4. Find The Hotspots

Prioritize areas such as:

- changed branching, special-case handling, or guard logic
- changed states, lifecycle handling, visibility, permissions, charging, allocation, or other rules
- changed failure handling, retry, compensation, fallback, or recovery behavior
- changed API contracts, schema boundaries, event payloads, or permission boundaries
- changed decision placement, or logic forced into the wrong layer without structural cleanup

Read [references/core-logic-signals.md](references/core-logic-signals.md) when useful.

## 5. Translate Hotspots Into Design Concerns

Sort findings into these four review buckets:

- `review deeply`: design judgment is central, or the change touches rules, core decision logic, system-critical control logic, responsibility placement, state transitions, or contracts
- `review if needed`: orchestration, non-trivial refactors, test changes that imply intent changes, or wiring near important boundaries
- `safe to skim`: mechanical edits, shallow wiring, behavior-preserving renames, generated refreshes
- `delegate to automation`: lint, codegen, import order, static checks, and other changes with no semantic difference

Useful ambiguous examples:

- moving logic from controller to service may be `review deeply` or `review if needed` depending on whether decision placement actually improved
- large test-only changes may be `review if needed` if they imply changed intent, or `safe to skim` if they are only follow-up updates
- helper extraction with fewer lines may still be `review deeply` if it obscures decision placement
- a new flag used as a local fix may be `review deeply` if it sidesteps proper state or exception design

Every `review deeply` hotspot must have an explicit reason. Anchor it in at least one of these:

- likely mismatch between intended design and implementation shape
- suspicious placement of decision logic
- workaround-shaped handling
- unstable decision placement or responsibility boundaries
- high damage if wrong

Read [references/review-target-prioritization.md](references/review-target-prioritization.md) and [references/workaround-smells.md](references/workaround-smells.md) when needed.

Translate from diff-language into design-review language. For example:

- not “this diff is large,” but “this responsibility placement is the issue”
- not “more branching was added,” but “this rule expression is the issue”
- not “this looks complicated,” but “this special-case handling may be a workaround rather than a proper extension”

Each hotspot should include at least these eight items:

- `target`: what to inspect
- `location`: where in the code the change lives
- `evidence`: which concrete diff facts support the concern
- `concern`: why it is a design-review concern
- `priority reason`: why it belongs in this review bucket
- `view`: the skill’s concern or hypothesis
- `question`: what the human should verify
- `recommended action`: what to do next

Granularity rules:

- `review deeply` must include all eight items
- `review if needed` must include at least `target / location / evidence / concern / priority reason / question`, with `view` and `recommended action` added when useful
- `safe to skim` and `delegate to automation` can be grouped

`evidence` must contain at least one concrete diff fact.

- a concrete changed condition, field, status, branch, retry rule, permission edge, payload change, or other diff fact

Do not use a file name or function name alone as evidence. Those belong in `location`.

## 6. Produce Review-Supporting Views And Actions

Views are not final judgments. They are review hypotheses meant to speed up human review.

How to write them:

- avoid false certainty
- make the concern or structural discomfort explicit
- do not assume the intended design if the evidence does not show it
- tie the view to the actual concern in the diff
- do not invent claims beyond the evidence

Recommended actions should lead naturally to a next step. Typical actions:

- `inspect deeply`: read code and design sources together
- `check against design sources`: compare with ADRs, design memos, or existing rules
- `consult a domain owner`: confirm rule intent or exception behavior
- `accept for now`: explicitly accept the risk and record follow-up if needed
- `suggest small follow-up cleanup`: propose a bounded cleanup inside the changed area

Use `direction of change` only as an extra lens when needed, especially if decision placement became clearer or if workaround hardening is a concern.

## 7. Add Visualization Only If It Helps

Visualization is a supporting tool. If you use it, prefer one view and add a second only when it clearly changes reviewer behavior.

### Pattern A: Structure Drift View

Use when the main need is to show where responsibility placement or dependency direction drifted.

Show:

- where decision logic now lives
- where logic escaped its proper responsibility
- where dependency reversal or boundary crossing appears

### Pattern B: Rule Fidelity View

Use when the main need is to show changed rule or state expression.

Show:

- changed rules or workflow steps
- branch, state-transition, or failure-path changes
- missing, duplicated, or patched-on rule handling

### Pattern C: Direction View

Use only as a supporting view when the reviewer needs help deciding whether decision placement became clearer or workaround hardening increased.

Show:

- `improved / neutral / regressed`
- places where decision placement became clearer
- places where ad hoc structure was hardened

Read the visualization section in [references/review-handoff-format.md](references/review-handoff-format.md) when useful.

## 8. Produce The Review Handoff

Use this structure:

- `review artifact contract`: diff, base, spec sources, exclusions, known gaps
- `review target selection`: grouped by `review deeply / review if needed / safe to skim / delegate to automation`
- `extracted design concerns`
- `agent view`
- `recommended reviewer action`
- `optional visualization`

Output limits:

- `review deeply`: at most 3 items
- `review if needed`: at most 5 items
- `safe to skim`: grouping allowed
- `delegate to automation`: grouping allowed

When the limit would be exceeded, group rather than enumerate everything.

Read [references/review-handoff-format.md](references/review-handoff-format.md) when useful.

For contrastive examples of good and weak handoffs, read [references/handoff-examples.md](references/handoff-examples.md).

## Output Rules

- do not give every changed area equal weight
- always pair `what to inspect` with `why it matters`
- tie concerns to business meaning, structural meaning, or control meaning
- do not say only “this looks complex”; say what makes reasoning or future change harder
- make the reviewer’s next step explicit
- include at least one `evidence` item for every hotspot
- include a `priority reason` for every hotspot
- if you recommend visualization, explain how it speeds judgment
- state uncertainty explicitly when confidence is limited

## Watch Outs

- do not treat large files as automatically high value
- do not give generated or mechanical changes the same weight as design-relevant changes
- do not miss structural regressions just because the code “works”
- do not mistake extra abstraction for better design
- do not call helper extraction alone an improvement
- do not demand full redesigns for every change
- do not make `direction of change` the main point every time

## Self-Check

Before responding, confirm:

- the design-review focus is clear
- the request is actually a fit for the skill
- the `review artifact contract` is present
- the hotspots in business logic, core decision logic, and system-critical control logic were actually extracted
- every deep-review hotspot has a design reason
- the four review buckets are preserved in the final output
- the item limits are respected
- every hotspot has `evidence`
- every hotspot has `priority reason`
- every hotspot includes a reviewer question
- the views support human judgment instead of replacing it
- the recommended actions lead to real next steps
- workaround signals were actually considered
- supporting lenses such as KISS, understandability, and changeability were used only when helpful
- `direction of change` was used only when it truly helped
- any visualization is judgment-supporting rather than decorative
