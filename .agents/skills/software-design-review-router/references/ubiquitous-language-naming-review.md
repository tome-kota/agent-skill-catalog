# Ubiquitous Language & Naming Review

## Purpose

Review whether the language used across code, docs, diagrams, and discussion expresses domain concepts honestly, consistently, and at the right level of abstraction.

## When to use

Use this reference when the main issue is overloaded vocabulary, misleading terms, inconsistent naming across contexts, or implementation-shaped words that obscure real domain meaning.

## Primary questions

- Which important concepts do not yet have stable names?
- Which current terms imply the wrong behavior or scope?
- Which words are carrying multiple meanings across contexts?
- Which names are technical artifacts rather than domain language?

## Review procedure

1. Identify the bounded context and audience for the language.
2. Extract the important concepts, states, operations, and constraints.
3. Classify naming problems by missing, misleading, overloaded, or implementation-shaped terms.
4. Propose narrower, more honest replacements.
5. Check whether the new wording works consistently across code, docs, diagrams, and conversation.

## Output cues

Produce a review that explains what is wrong with the current wording, what better language should replace it, and which boundaries that wording protects.

## Review stance

Good naming is not decoration.

Names should help a reader answer:

- what exists here?
- what can happen?
- what distinguishes this concept from nearby ones?
- what should stay true?

Do not optimize for polish alone. Optimize for honest understanding.

## Naming problem types

### Missing term

A concept exists in behavior or rules, but no explicit name exists for it.

Signals:

- repeated logic with no shared label
- a long function or paragraph hiding a reusable concept
- multiple parameters always traveling together

### Misleading term

A word suggests a meaning or behavior that is not actually true.

Signals:

- a method name promises one thing and does another
- a type sounds domain-level but is really technical
- a status name hides lifecycle timing or side effects

### Overloaded term

One word is doing too much.

Signals:

- the same label means different things in different places
- a cross-context concept is being forced under one shared name
- discussions require repeated verbal clarification

### Implementation-shaped term

The word describes how the system is built rather than what the concept means.

Signals:

- storage-driven names
- transport-driven names
- framework lifecycle names used as domain vocabulary

## Rewrite strategy

When fixing a name:

1. decide what concept is actually being named
2. remove false implications
3. add the distinction that matters
4. check whether the new term still works in natural team conversation

Prefer names that are:

- honest
- specific enough to distinguish nearby concepts
- stable across implementation changes
- natural inside the bounded context

## Context discipline

The same term does not need to mean the same thing everywhere.

Allow different contexts to use different words when:

- they protect different invariants
- they care about different attributes
- they operate at different abstraction levels

Unifying vocabulary across contexts is only good when the meaning is genuinely shared.

## Common naming traps

### Vocabulary drift

The same concept slowly acquires different names in code, docs, and discussions.

### Premature elegance

The wording sounds polished but hides important behavior or limits.

### Technical masquerade

A transport or storage term is treated as if it were the real domain concept.

### Scope inflation

A broad label absorbs several smaller concepts because splitting them feels inconvenient.

## Deep checks

Use this section when the initial review suggests that naming problems are causing model drift or review confusion.

### What to probe further

- whether the same term changes meaning between code, docs, and conversation
- whether multiple concepts are being hidden behind a polished but overly broad label
- whether technical names are defining the domain by accident
- whether context differences are being flattened in the name of consistency

### Strong warning signs

- reviewers repeatedly need verbal explanation for the same term
- one name is doing the work of concept, state, and workflow stage at the same time
- a rename sounds cleaner but still does not reveal the behavior
- the team shares one word across contexts because splitting it feels socially awkward

### Review prompts

- What misunderstanding does this term invite?
- Which distinct concept deserves its own name?
- Does this word belong to the domain or to the implementation?
- Where should the same label stop being reused?

## Recommendation rules

- Add names for concepts that are already present in behavior.
- Split terms that carry more than one business meaning.
- Rename terms that imply false behavior or scope.
- Keep technical implementation labels out of the core domain language unless the technology is itself the domain.
- Prefer context-qualified names over one misleading universal term.

## Output shape

Structure findings around:

- current term
- naming problem type
- recommended term
- why the change improves understanding
- where the rename should apply

## Acceptance checks

A strong review from this reference should:

- expose missing and overloaded concepts
- improve the honesty of the vocabulary
- reduce implementation-shaped naming
- preserve bounded-context distinctions
- produce naming recommendations people can actually use in conversation and code
