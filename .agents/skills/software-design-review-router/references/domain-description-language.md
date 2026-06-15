# Domain Description Language

## Purpose

Review or shape the language used to describe a domain so that concepts, operations, states, and rules are expressed precisely enough to support consistent design and implementation.

## When to use

Use this reference when the main problem is ambiguous business meaning, overloaded terminology, weak state modeling, or domain behavior being described in technical rather than business language.

## Primary questions

- Which concepts actually matter in this domain?
- Which distinctions are getting collapsed into vague or overloaded terms?
- Which operations change business meaning rather than just storage?
- Which states, transitions, and constraints need to be made explicit?

## Review procedure

1. Extract the core vocabulary of the domain.
2. Separate concepts, actions, states, and constraints.
3. Identify the distinctions that the current language is hiding.
4. Rework terms so business meaning is carried directly in the language.
5. Check that the resulting language makes behavior, rules, and illegal states easier to see.

## Output cues

Produce a review that clarifies domain vocabulary, highlights critical distinctions, and recommends wording that makes business behavior easier to reason about.

## Review stance

This is not a programming-language design exercise.

The goal is not elegance or clever naming. The goal is to express domain meaning with enough precision that:

- rules are visible
- states are legible
- behavior is discussable
- invalid situations are harder to hide

## Vocabulary layers

Review the language in four layers:

- concepts
- operations
- states and transitions
- constraints

Weak domain language usually mixes these together until nothing is precise.

## Concept checks

Extract the real nouns of the domain.

Look for:

- concepts that are present but unnamed
- one term covering several distinct things
- technical names standing in for business concepts
- data containers treated as if they were domain concepts

Prefer terms that describe meaning, not storage or transport.

## Operation checks

Domain operations should describe what changes in the business world.

Weak signals:

- create
- update
- process
- manage
- handle

Stronger signals usually name the domain effect, such as:

- reserve
- approve
- reject
- expire
- settle
- reconcile

If an operation changes meaning, give it a meaningful verb.

## State and transition checks

Review whether the language makes it obvious:

- which states exist
- what transitions are allowed
- which transitions are irreversible
- which conditions authorize the transition

Bad state language often hides behavior inside generic mutation.

## Constraint checks

Treat business rules as part of the language, not implementation detail.

Look for:

- exclusivity rules
- temporal rules
- quantity limits
- authorization conditions
- consistency conditions

If a constraint is important to correctness, the domain language should help people talk about it directly.

## Language smells

### Technical leakage

The domain is being described in terms of DTOs, endpoints, tables, or framework artifacts.

### Semantic compression

Several different ideas are being folded into one convenient word.

### Hidden transition

State changes are happening, but the language has no explicit way to describe them.

### Workflow-shaped vocabulary

Terms describe a process implementation rather than the business meaning of the step.

### Generic action wording

The language names mechanics instead of intent.

## Deep checks

Use this section when the initial review suggests that the domain vocabulary is too vague to support stable design decisions.

### What to probe further

- whether state and operation words are being mixed together
- whether important constraints are invisible in the available vocabulary
- whether several business distinctions are being compressed into one convenient term
- whether technical artifacts are silently shaping the language of the domain

### Strong warning signs

- many core verbs are generic and transport-like
- one status word carries timing, authorization, and validity at once
- teams can describe the flow but not the actual domain transitions
- the implementation can violate a business rule without the vocabulary making it obvious

### Review prompts

- Which business distinction disappears if this term stays vague?
- What operation name would make the domain effect explicit?
- Which rule matters but currently has no clean language?
- What state transition is happening without being named?

## Consistency checks

After proposing improved terms, verify that:

- the same term means the same thing inside one context
- different meanings are not forced under one shared label
- state names and operation names fit together
- constraints do not contradict the wording
- documentation, diagrams, and code can use the same core vocabulary

## Recommendation rules

- Split terms when one word is hiding several meanings.
- Rename operations when the current verb hides business effect.
- Make states explicit when mutation currently conceals transitions.
- Keep technical labels out of the core domain vocabulary unless the technology is itself part of the domain.
- Prefer a smaller precise vocabulary over a larger vague one.

## Output shape

Structure findings around:

- important terms and what is wrong with them
- missing distinctions that need names
- operations whose wording hides behavior
- states or constraints that should be made explicit

## Acceptance checks

A strong review from this reference should:

- identify the domain concepts that matter
- expose hidden semantic distinctions
- improve how operations and states are described
- reduce technical leakage in domain wording
- make constraints easier to express and discuss
