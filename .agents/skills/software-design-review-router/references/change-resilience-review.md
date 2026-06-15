# Change Resilience Review

## Purpose

Review whether a design or change can absorb likely future modification without hardening ad hoc workarounds or overbuilding for hypothetical futures.

## When to use

Use this reference when the main concern is that a plan, implementation, or proposed structure feels too temporary, too workaround-heavy, or too speculative to hold up under future change.

## Primary questions

- Does this proposal make the next likely change easier or harder?
- Is variability being localized, or spread through exceptions and flags?
- Is a short-term workaround being turned into permanent structure?
- Is the design adding abstraction for real pressure or imaginary flexibility?

## Review procedure

1. Identify the next likely changes that matter.
2. Trace where those changes would land in the current proposal.
3. Check whether transition logic or edge cases are shaping the main design.
4. Distinguish proven extension pressure from speculative generalization.
5. Recommend the smallest durable structure that improves future change safety.

## Output cues

Produce a review that explains whether the design improves change resilience, where it increases future friction, and whether it is under- or over-structured for the change pressure that actually exists.

## Review stance

Do not equate flexibility with quality.

The goal is not maximum extensibility. The goal is to make credible future changes:

- cheaper
- safer
- more local

without inventing infrastructure for futures nobody has earned yet.

## Likely-change check

Before judging abstraction, identify:

- which follow-on changes are actually plausible
- which pressures are already recurring
- which constraints are stable
- which edge cases are temporary

If the proposal cannot name the future change it helps, its flexibility may be unproven.

## Variability checks

Look for where change pressure is being absorbed.

Good signs:

- one local policy point
- clear extension seam where variation is real
- stable concepts kept stable

Bad signs:

- repeated conditionals in several places
- flags controlling unrelated behavior
- migration logic leaking into steady-state structure
- option surfaces with no current owner

## Workaround hardening

Be suspicious when:

- temporary compatibility logic becomes the dominant abstraction
- rare cases shape the common model
- current implementation pain is being embedded as permanent design

A workaround can be necessary. The problem starts when the workaround becomes the architecture.

## Speculative generalization

Ask:

- how many real consumers need this abstraction now?
- what repeated pattern proves the variability?
- would the simpler design still be easy to extend after a second real use case appears?

Generalize after pressure is visible, not before.

## Common change-resilience traps

### Workaround as foundation

A transitional solution is promoted into the base design.

### Local patch, distributed cost

Today's simple shortcut creates tomorrow's synchronized edits.

### Exception-path architecture

Rare behavior defines the mainline model.

### Abstraction before demand

Indirection is added without real variation or repeated need.

### Option surface inflation

Public surface area grows faster than real business scenarios.

## Deep checks

Use this section when the initial review suggests that the design may be surviving today's problem by making tomorrow's changes harder.

### What to probe further

- whether temporary compatibility or exception logic is becoming the steady-state model
- whether one real use case is being used to justify broad abstraction
- whether the next likely change would still scatter across modules
- whether a claimed extension seam has more imagined consumers than real ones

### Strong warning signs

- strategy, plugin, or mode systems appear before real variation exists
- feature flags and conditionals keep accumulating without being retired
- a workaround is being promoted because removing the root cause feels harder
- the design claims flexibility but still requires broad edits for the next realistic change

### Review prompts

- Which future change is this abstraction actually buying?
- What temporary concern is becoming permanent structure?
- Would a smaller design make the second real use case easier or harder?
- Where is change pressure spreading instead of being absorbed?

## Recommendation rules

- Keep temporary concerns isolated from steady-state structure.
- Introduce abstraction only where recurring variation is visible.
- Prefer local complexity over system-wide indirection when change pressure is still narrow.
- Collapse premature frameworks that exist only to feel extensible.
- Add structure when known variability is already scattering across the codebase.

## Output shape

Structure findings around:

- the next likely changes that matter
- where those changes would land
- whether the design is too rigid or too abstract
- what smallest structural adjustment improves resilience

## Acceptance checks

A strong review from this reference should:

- name the next likely changes that matter
- distinguish workaround hardening from justified structure
- detect speculative extensibility
- make an explicit YAGNI-aware judgment
- recommend the smallest durable structure
