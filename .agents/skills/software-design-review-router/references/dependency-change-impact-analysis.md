# Dependency & Change Impact Analysis

## Purpose

Review the real blast radius of a proposed change across code, contracts, data, runtime behavior, operations, tests, and ownership boundaries.

## When to use

Use this reference when the main risk is hidden consumers, migration coupling, compatibility fallout, rollout danger, or a change that appears local but may propagate widely.

## Primary questions

- What actually depends on this change?
- Which consumers, producers, contracts, or operational assumptions are easy to miss?
- What migration, rollout, rollback, or verification work is implied?
- Where is the current impact analysis too optimistic?

## Review procedure

1. Define the change boundary clearly.
2. Map the dependency surfaces it touches.
3. Look for hidden or indirect consumers and producers.
4. Separate compatibility concerns from rollout safety concerns.
5. Review migration shape, change order, and evidence.
6. State the real blast radius and what must happen to make the change safe.

## Output cues

Produce a review that makes hidden dependencies, migration shape, operational risk, and residual uncertainty explicit.

## Review stance

Do not confuse "small diff" with "small impact."

Many dangerous changes are dangerous because:

- dependency surfaces are invisible
- ownership is unclear
- rollout order matters
- tests cover only part of the real dependency graph

## Dependency surfaces

Review across:

- modules and libraries
- APIs and internal contracts
- database schemas and data shape
- events and message formats
- configuration and feature flags
- jobs, pipelines, and scheduled processes
- observability and operations
- team ownership boundaries

## Hidden-dependency checks

Look for:

- undocumented consumers
- consumers reading data through side paths
- copied schemas or DTOs
- operational scripts depending on the old behavior
- tests that are the only current proof of safety
- rollout steps that assume a specific deployment order

## Migration checks

Ask:

- is this additive, mutating, or destructive?
- what compatibility window is needed?
- can new and old behavior coexist safely?
- what cutover signal will show readiness?
- what cleanup step exists after migration?

Migration is part of design, not postscript.

## Compatibility versus safety

Separate these questions:

- can old and new parties still communicate?
- can the system be changed in this order without incident?

A change may be technically compatible and still unsafe to roll out.

## Common impact traps

### Hidden consumer

The change breaks an actor nobody accounted for.

### Implicit contract drift

The shape still compiles, but meaning has shifted.

### Rollout order hazard

The change only works if several moving parts happen in the right sequence.

### Migration without reconciliation

The design changes shape, but not how existing data or behavior will be brought forward.

### Ownership vacuum

The change crosses boundaries without clear responsibility for validation and recovery.

## Deep checks

Use this section when the initial review suggests that the blast radius is larger or less understood than the proposal admits.

### What to probe further

- whether hidden consumers exist outside the main code path or main team
- whether migration steps assume a rollout order that has not been named
- whether rollback is possible once data or contracts have moved forward
- whether operational scripts, dashboards, or support procedures depend on the old behavior

### Strong warning signs

- the change is described as additive but cutover still has a sharp edge
- cleanup exists only implicitly and nobody owns it
- compatibility is discussed, but verification is vague
- different teams assume someone else will validate the dangerous step

### Review prompts

- Who breaks if this change lands in the wrong order?
- What dependency is easy to miss because it is not in the normal call graph?
- What must be true before cleanup is safe?
- If the change is wrong in production, how exactly do we step back?

## Recommendation rules

- Narrow the change if the dependency footprint is not understood.
- Add compatibility stages when destructive change is currently too direct.
- Insert verification and observability before the irreversible step.
- Name the deployment and rollback order explicitly when order matters.
- Treat unclear ownership as a design risk, not just a process issue.

## Output shape

Structure findings around:

- the real change boundary
- dependency surfaces touched
- hidden or weakly understood dependencies
- migration and rollout implications
- the safest next move

## Acceptance checks

A strong review from this reference should:

- define the real blast radius
- find dependencies beyond the obvious call graph
- distinguish compatibility from rollout safety
- evaluate migration and rollback shape
- identify missing ownership or evidence
