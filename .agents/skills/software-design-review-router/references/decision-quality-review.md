# Decision Quality Review

## Purpose

Review whether a software design decision is being made for the right problem, with the right criteria, and with a realistic path to execution.

## When to use

Use this reference when the main question is not "will this work at all?" but "is this the right decision for this context?" This is especially useful for ADR critique, architecture options, major refactors, boundary changes, and AI-generated plans that look persuasive but may be optimizing the wrong thing.

## Primary questions

- What problem is actually being solved?
- Which evaluation axis is truly dominant in this situation?
- Which constraints are real, and which are assumed or unexamined?
- Were meaningful alternatives considered, or did the first plausible answer become the decision?
- Can the team actually adopt, operate, and evolve this choice?

## Review procedure

1. Clarify the decision under review and the decision it is pretending to be.
2. Identify the dominant evaluation axis for this context.
3. Separate facts, assumptions, and copied best-practice reasoning.
4. Check whether real alternatives were considered.
5. Test whether the proposal is organizationally and operationally survivable.
6. State the most important decision distortions and the best next move.

## Output cues

Produce a review that explains what is being optimized, what is being ignored, why that mismatch matters, and what decision direction is strongest now.

## Review stance

Judge the quality of the decision, not the aesthetics of the solution.

Do not give extra credit for:

- novelty
- architectural fashion
- abstraction density
- "enterprise-ready" appearance
- AI-generated confidence

A decision can be technically clever and still be poor.

## Decision framing checks

Before reviewing details, identify:

- the actual choice being made
- the boundary of the decision
- the expected lifespan of the decision
- the main stakeholders
- the failure mode that matters most
- the cost of being wrong

Red flags:

- the proposal solves several different problems at once
- the review focuses on implementation shape before decision criteria
- "future-proofing" is used without naming a concrete future change
- urgency is used to avoid clarifying the problem

## Evaluation axes

The important question is not "which axes exist?" but "which axis should dominate here?"

Common dominant axes:

- purpose fit
- constraint fit
- technical feasibility
- organizational feasibility
- long-term evolvability
- short-term containment
- compatibility stability
- risk reduction

Use one axis as the driver. Treat the others as constraints or tradeoffs.

## Information quality checks

Distinguish:

- measured facts
- strong evidence
- operating assumptions
- aspirational claims
- aesthetic preferences

Be skeptical of reasoning built on:

- benchmarkless performance claims
- "industry standard" appeals without context
- framework defaults treated as architecture
- AI-generated plans with no source-of-truth evidence

## Alternative quality checks

Ask whether the review compares:

- at least one simpler option
- at least one option with lower operational burden
- at least one option that optimizes a different dominant axis

Weak alternative analysis often looks like:

- one real option plus one strawman
- one option with cosmetic variations
- one default selected before tradeoffs are named

## Organizational fit checks

Test whether the choice matches:

- team skill level
- release and rollback capability
- debugging reality
- on-call burden
- ownership clarity
- expected maintenance discipline

A design that requires a stronger organization than the one that exists is not ready, even if it is technically valid.

## Common decision distortions

### Wrong problem, right-looking solution

The proposal is internally coherent, but aimed at the wrong problem.

Signals:

- solution detail is richer than problem framing
- the stated pain and the optimized metric do not match
- a local complaint is driving a strategic redesign

### Criteria drift

The team starts with one goal and gradually judges the proposal on another.

Signals:

- reliability problem becomes a technology prestige debate
- compatibility problem becomes a performance debate
- emergency mitigation becomes a platform rewrite

### Evidence theater

The proposal feels well-argued but rests mostly on confidence and vocabulary.

Signals:

- repeated claims without concrete system evidence
- generic references standing in for local facts
- no clear distinction between knowns and unknowns

### Alternative collapse

The first credible option becomes the only option.

Signals:

- no honest simpler candidate
- no option optimized for operational reality
- no explicit tradeoff against the current state

### Adoption fantasy

The design assumes a stronger execution culture than the organization actually has.

Signals:

- rollout, monitoring, and debugging are missing
- cross-team coordination is assumed to be easy
- the proposal requires discipline that current systems do not show

## Deep checks

Use this section when the initial review suggests the decision may be well-presented but poorly grounded.

### What to probe further

- whether the stated problem and optimized metric still match after reading the whole proposal
- whether the dominant axis changes between problem framing, comparison, and recommendation
- whether the proposal relies on local constraints while pretending to solve a strategic problem
- whether adoption complexity is being treated as an implementation detail instead of a decision constraint

### Strong warning signs

- the proposal explains implementation shape in more detail than decision criteria
- the "alternative analysis" is really one option plus weaker variants
- the most confident parts of the argument are also the least evidenced
- organizational burden appears only near the end, after the architecture is already chosen

### Review prompts

- What decision is this proposal actually making?
- If the dominant axis changed, when did it change and why?
- What simpler or more local option deserves a fair comparison?
- What part of the recommendation depends on execution discipline the team does not consistently have?

## Recommendation rules

- If the framing is wrong, fix the framing before comparing solutions.
- If the dominant axis is unclear, make it explicit before discussing detail.
- If evidence is weak, narrow the decision or insert validation steps.
- If alternatives were not explored, treat confidence as premature.
- If organizational fit is weak, simplify before scaling ambition.

## Output shape

Structure findings around:

- the real decision being made
- the dominant evaluation axis
- the strongest distortion or blind spot
- the most credible alternative or reframing
- the best next step

## Acceptance checks

A strong review from this reference should:

- identify the actual decision, not just the proposed implementation
- name the dominant evaluation axis
- separate facts from assumptions
- show whether real alternatives were considered
- make organizational fit part of the judgment
- produce a recommendation that is narrower and clearer than the original debate
