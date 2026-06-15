# Non-Functional Budget Governance

## Purpose

Review whether a design fits explicit budgets for performance, capacity, reliability, and cost.

## When to use

Use this reference when a proposal may be constrained by latency, throughput, concurrency, storage growth, compute cost, external API volume, or operational headroom.

## Primary questions

- Which non-functional budget actually governs this decision?
- What load shape and growth assumptions matter?
- Where are the multiplicative cost or scale risks?
- What evidence is needed before claiming the design is safe?

## Review procedure

1. Define the budget that matters.
2. Build a realistic load and growth model.
3. Identify the strongest budget breakers.
4. Review scaling, storage, cost, and operability consequences.
5. State what validation or redesign is required.

## Output cues

Produce a review that names the dominant budget, the main assumptions, the biggest constraint risks, and the minimum validation needed for confidence.

## Review stance

Treat non-functional requirements as design constraints, not later tuning tasks.

Be suspicious of:

- "it should scale"
- "we can optimize later"
- "cloud infrastructure will absorb it"

## Budget framing checks

Define:

- critical user journeys
- critical workloads
- time horizon
- environments
- latency targets
- throughput or concurrency expectations
- storage or cost ceilings

Vague adjectives are not budgets.

## Load model checks

Review:

- average load
- peak load
- burst behavior
- fan-out
- retries
- payload size
- retention and growth

Ranges are acceptable. Unnamed assumptions are not.

## Budget breaker checks

Look for multiplicative risk in:

- repeated network calls
- joins and scans
- cache-miss-heavy paths
- queue backlogs
- large object movement
- repeated serialization
- lock contention
- hot partitions

## Operability checks

Ask:

- how will budget violation be detected?
- what saturates first?
- what degrades under pressure?
- what recovery path exists?

A design that fits only in the happy path is not within budget.

## Common non-functional traps

### Adjective-based design

The proposal uses words like "fast" or "small-scale" instead of real limits.

### Hidden multiplier

The design ignores fan-out, retries, bursts, or growth compounding.

### Cost blind spot

The proposal can work technically, but no one has named the compute, storage, or external dependency bill.

### Operability gap

The design cannot show when it is approaching failure.

## Deep checks

Use this section when the initial review suggests that the proposal is relying on rough intuition instead of a credible budget model.

### What to probe further

- whether the load model ignores burst behavior, retries, or fan-out
- whether cost is acceptable only under average traffic, not peak conditions
- whether the first saturation point is known
- whether observability exists for the specific budget that matters

### Strong warning signs

- req/s is named, but burst duration or payload growth is not
- latency is discussed without downstream dependency behavior
- scaling claims assume cache hit rates or concurrency patterns with no evidence
- the design can fail slowly and expensively before anyone notices

### Review prompts

- What is the strongest hidden multiplier in this path?
- What assumption would most damage the budget if it is wrong?
- What saturates first under peak load?
- How will the team know the design is approaching its limit?

## Recommendation rules

- Define the budget before debating architecture shape.
- Narrow the design when the load model is still fiction.
- Add measurement or pressure testing before claiming budget fit.
- Simplify where performance complexity exists without a real budget need.
- Redesign where the dominant budget is being violated by structure, not implementation detail.

## Output shape

Structure findings around:

- dominant budget
- key assumptions
- strongest budget breakers
- required validation or redesign

## Acceptance checks

A strong review from this reference should:

- identify the real budget constraint
- expose hidden multipliers
- separate facts from assumptions
- include operability in the judgment
- recommend validation when evidence is weak
