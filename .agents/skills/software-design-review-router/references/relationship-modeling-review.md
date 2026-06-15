# Relationship Modeling Review

## Purpose

Review whether relationships between concepts, entities, aggregates, and contexts express real business meaning, clear ownership, and sustainable consistency boundaries.

## When to use

Use this reference when the main issue is how concepts should relate, who owns consistency, where lifecycle dependency exists, or whether a modeled relationship is accidental rather than meaningful.

## Primary questions

- Why does this relationship exist in business terms?
- Which side owns the invariant or pays the consistency cost?
- Does the relationship imply containment, coordination, reference, or merely lookup?
- Is navigation required, or is it being introduced for convenience?

## Review procedure

1. Identify the meaningful concepts before connecting them.
2. Explain what each proposed relationship means.
3. Evaluate ownership, lifecycle dependency, and consistency implications.
4. Check whether navigation and containment are truly required.
5. Recommend the lightest relationship model that preserves business meaning.

## Output cues

Produce a review that explains what each important relationship means, what boundary cost it creates, and what relationship shape best fits the domain.

## Review stance

Do not start from:

- tables
- foreign keys
- screen navigation
- ORM convenience
- reporting shortcuts

Start from business meaning, then decide what structure is deserved.

## Concept-first check

Before judging relationships, confirm that the underlying concepts are independently meaningful.

Weak relationship modeling often begins by connecting poorly understood concepts too early.

Ask:

- what exists on its own?
- what changes on its own?
- what has its own lifecycle?
- what is only meaningful through another concept?

## Meaning check

For each relationship, state:

- what it means
- why it exists
- what rule or coordination need it reflects

If that cannot be explained without implementation language, the relationship may be accidental.

## Ownership and lifecycle checks

Test:

- which side defines the invariant
- whether one side can exist without the other
- whether one side creates, retires, or governs the other
- whether the relation is stable or frequently changing

Ownership is about responsibility for correctness, not just where a pointer lives.

## Consistency checks

Ask:

- must both sides update atomically?
- can the relationship be eventually consistent?
- is this relation inside one aggregate or across a boundary?
- who pays the coordination cost if the relation changes?

Do not turn every business association into one consistency boundary.

## Navigation checks

Navigation is not free.

Ask:

- who genuinely needs traversal?
- is the traversal for business behavior or developer convenience?
- can identity or lookup be enough?

Many overgrown models come from assuming every relationship must be traversable in both directions.

## Common modeling traps

### Database-shaped relationship

The model copies storage structure instead of domain meaning.

### Screen-driven relationship

The UI needs one combined view, so the domain model is forced to mirror the screen.

### Ownership blur

The model relates two concepts but does not say who owns consistency.

### Aggregate inflation

Concepts are pulled into the same boundary because they are related, not because they share invariants.

### Reference as false containment

An external relationship is modeled as if one concept fully contains the other.

### Bidirectional by default

Two-way navigation is introduced without a real need.

## Deep checks

Use this section when the initial review suggests that related concepts are being connected too eagerly or at the wrong boundary.

### What to probe further

- whether ownership and lifecycle are being inferred from storage rather than business meaning
- whether aggregate boundaries are absorbing coordination that could remain external
- whether navigation is being added for convenience instead of behavior
- whether one concept is falsely modeled as contained by another

### Strong warning signs

- the model cannot explain who owns consistency
- many-to-many or bidirectional relationships appear before meaning is clarified
- the UI needs a combined view, so the domain model is forced to become a combined structure
- two concepts are grouped together mainly because they often appear on the same screen or request

### Review prompts

- What invariant justifies this relationship shape?
- Could these concepts remain independent with reference or lookup?
- Who pays the consistency cost if the relationship changes?
- What relationship is being modeled here that the business itself does not describe?

## Recommendation rules

- Keep relationships explicit only when they carry real business meaning.
- Use reference instead of containment when the concepts are independent.
- Keep ownership local to the side that protects the invariant.
- Avoid widening aggregate boundaries just because concepts collaborate.
- Prefer lookup or identifier relationships when full navigation adds cost without meaning.

## Output shape

Structure findings around:

- the important relationship decisions
- what each relationship actually means
- ownership and lifecycle implications
- consistency consequences
- the recommended relationship shape

## Acceptance checks

A strong review from this reference should:

- explain the meaning of the relationship in business terms
- identify who owns consistency
- distinguish containment from coordination
- avoid modeling convenience as domain truth
- recommend the smallest relationship shape that preserves meaning
