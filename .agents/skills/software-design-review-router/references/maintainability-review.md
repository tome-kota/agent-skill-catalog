# Maintainability Review

## Purpose

Review how a design or code change affects future changeability, readability, and testability.

## When to use

Use this reference primarily as a supporting lens when maintenance cost could change the overall design recommendation. Use it as a primary lens only when the task is explicitly about maintainability rather than architecture.

## Primary questions

- Will future changes be local and understandable?
- Is the structure easier for another engineer to modify safely?
- Are side effects, boundaries, and responsibilities easier to see?
- Does the design improve or reduce testability?

## Review procedure

1. Check how change would land in the new structure.
2. Review readability and consistency of abstraction.
3. Review side effects, dependencies, and test seams.
4. Identify maintenance costs introduced by the change.
5. State whether the maintainability impact should alter the broader recommendation.

## Output cues

Produce a review that focuses on future maintenance cost, boundary clarity, and testability rather than formatting or personal style.

## Review stance

This reference is about change cost, not correctness and not code style.

Ignore:

- formatting preferences
- language-war debates
- micro-optimizations without evidence
- purely personal conventions

## Changeability checks

Look for:

- clear responsibility boundaries
- small and predictable edit surfaces
- minimal speculative abstraction
- stable APIs and constructors
- validation in consistent places

Worry when:

- one change will require edits in several unrelated places
- abstractions were added before repeated need exists
- responsibilities blur across layers

## Readability checks

Look for:

- names that explain intent
- consistent abstraction level
- code that reveals its side effects
- explicit domain concepts

Worry when:

- one unit mixes several ideas
- names hide behavior
- generic wrappers conceal the important logic

## Testability checks

Look for:

- replaceable dependencies
- isolated side effects
- predictable behavior
- clear seams for unit and integration testing

Worry when:

- the only way to validate behavior is through broad end-to-end flows
- invalid states can be assembled freely
- important logic is inseparable from infrastructure

## Common maintenance traps

### Premature abstraction

Indirection exists before repeated need.

### Boundary leakage

Too much domain or infrastructure knowledge crosses layer boundaries.

### Hidden side effects

The code does more than the structure admits.

### Test-hostile structure

Important logic cannot be exercised without broad integration setup.

## Deep checks

Use this section when the initial review suggests that future edits may be more expensive than the code currently appears.

### What to probe further

- whether one likely follow-up change would touch several unrelated layers
- whether abstractions are hiding the real policy or real side effects
- whether tests can validate important behavior without broad environment setup
- whether readability problems are actually symptoms of responsibility confusion

### Strong warning signs

- a small behavior change requires editing constructors, mappers, services, and tests together
- helper layers exist mainly to avoid naming the real responsibility
- side effects are easy to trigger but hard to notice from the structure
- the safest test is still an expensive end-to-end flow

### Review prompts

- Where would the next likely edit land?
- Which unit is carrying more than one responsibility?
- What side effect is not obvious from the interface?
- What test seam is missing that would make this change safer next time?

## Recommendation rules

- Collapse abstraction that does not yet earn its cost.
- Clarify responsibility when one unit is doing several jobs.
- Move side effects toward clearer boundaries.
- Prefer smaller honest structures over generic ones with unclear payoff.

## Output shape

Structure findings around:

- the maintenance cost being introduced or removed
- where future edits will concentrate or scatter
- how readability or testability is affected
- whether this should change the larger design judgment

## Acceptance checks

A strong review from this reference should:

- explain how future changes would land
- identify readability or boundary problems that matter
- evaluate testability as part of maintainability
- avoid collapsing into style-only commentary
