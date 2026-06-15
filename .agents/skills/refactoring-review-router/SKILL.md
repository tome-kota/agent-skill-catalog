---
name: refactoring-review-router
description: >-
  Use this skill when a refactoring request needs a structural decision or staged safety plan involving state-space complexity, unclear responsibility boundaries, or dependency-impact risk. Route the work to exactly one primary refactoring lens and add a secondary lens only when it materially changes the plan. Do not use for style-only cleanup, ordinary small implementation tasks, framework tutorials, purely local bug fixes, or broad architecture review that is not centered on a refactoring move.
---

# Refactoring Review Router

Route a refactoring request to the smallest useful diagnostic lens, then synthesize one practical refactoring plan.

This is the single entry skill for practical refactoring review. The detailed methods live in `references/` and should be loaded only after selecting the relevant lens.

Default behavior:

1. Pick exactly one primary lens.
2. Add at most one secondary lens when it materially changes the plan.
3. Read only the selected reference files.
4. Return one integrated refactoring plan, not separate mini-reviews.

## When To Use

Use this skill when the user wants to:

- make a structural refactoring decision before moving code
- produce a safe staged refactoring plan for a risky code area
- make code safer to change before adding a feature that would otherwise force structural movement
- understand why a code area is hard to modify when the answer affects how refactoring should proceed
- untangle modes, flags, transitions, or derived state
- separate mixed responsibilities or clarify where logic belongs
- reduce blast radius, dependency coupling, or migration risk
- review an AI-generated refactoring plan or code change from a refactoring-design perspective

## When Not To Use

Do not use this skill when the task is primarily:

- formatting, linting, mechanical rename, or style-only cleanup
- implementing a clearly scoped feature with no refactoring decision
- fixing a syntax or type error
- answering a framework or library how-to question
- debugging one isolated defect with no structural concern
- broad architecture critique that is not about a refactoring move

If the task is small enough that ordinary implementation is clearly safe, do not make it heavy.

If the user is only asking for routine implementation help and no structural refactoring judgment is needed, do not trigger this skill just because the code is imperfect.

## Lens Selection

Choose one dominant concern.

### State-Space Complexity

Read [references/state-space-refactoring.md](references/state-space-refactoring.md) when the hard part is:

- modes, flags, statuses, transitions, selected entities, permissions, or feature flags
- async loading/error/success/retry/optimistic state
- validation, dirty state, submit state, or operation-specific validity
- duplicated or derived state
- invalid, impossible, or ambiguous combinations
- screens/forms/workflows where visibility, editability, action availability, and validation drift apart

Short version:

```text
state, modes, transitions, invalid combinations, derived state
```

### Responsibility-Boundary Confusion

Read [references/responsibility-boundary-refactoring.md](references/responsibility-boundary-refactoring.md) when the hard part is:

- code changes for too many unrelated reasons
- logic has no obvious home
- rendering, validation, authorization, policy, orchestration, formatting, persistence, mapping, or integrations are mixed together
- helpers, hooks, controllers, services, managers, composables, or shared modules became dumping grounds
- visual similarity or framework placement is hiding the real reason for change
- decision logic, operation validation, action availability, workflow orchestration, or display concerns need clearer ownership

Short version:

```text
change reasons, semantic ownership, responsibility placement
```

### Dependency-Impact Risk

Read [references/dependency-impact-refactoring.md](references/dependency-impact-refactoring.md) when the hard part is:

- changing one shared type, schema, API, DTO, module, event, helper, or contract may break many consumers
- hidden consumers or indirect dependencies are unclear
- migration must be staged
- old and new behavior must coexist
- domain logic depends directly on transport, persistence, framework, vendor APIs, or shared implementation details
- compile success is not enough to prove safety

Short version:

```text
dependency direction, consumers, blast radius, staged migration
```

## Secondary Lens Rules

Add a secondary lens only when it changes the sequencing or risk controls.

- State-space primary + responsibility secondary: use when modes and transitions are clear enough, but rule ownership or display/workflow boundaries decide the safe extraction order.
- Responsibility primary + state-space secondary: use when mixed responsibilities are driven by tangled mode, permission, or validation combinations.
- Dependency primary + responsibility secondary: use when reducing blast radius requires moving logic to a clearer owner.
- Responsibility primary + dependency secondary: use when moving a responsibility would affect many consumers or shared contracts.
- State-space primary + dependency secondary: use rarely, when the state model is serialized, shared, persisted, or consumed externally.

Do not include all three lenses by default. If all three seem relevant, choose the one that makes the first safe step possible and mention the others as follow-up checks.

When the primary lens is not obvious, break ties with these questions:

1. Which lens most changes the first safe move?
2. If we guess wrong, which axis creates the largest damage: behavioral confusion, ownership drift, or migration blast radius?
3. Is the user mainly asking how to model behavior or state transitions, how to place logic, or how to change shared contracts safely?

Use those answers to pick the primary lens. Keep the others as supporting notes unless they materially change sequence or verification.

## Workflow

1. Frame the refactoring problem.
   Identify the code area, intended change, pain symptoms, observable behavior, and current risk. If the target is unclear, inspect enough code to name the likely target.

2. Select the primary lens.
   Use the lens selection rules above. State the dominant concern in one sentence.

3. Load the selected reference.
   Read only the primary reference first. Read one secondary reference only if it materially changes the plan.

4. Inspect before proposing movement.
   When code is available, search and read the implementation, nearby tests, callers, related helpers, routes, schemas, and contracts as required by the selected reference. If code is unavailable or partial, do not invent evidence; produce assumptions, missing evidence, and a provisional plan instead.

5. Produce one integrated plan.
   Use the output contract from the primary reference. Add only the secondary concerns that affect sequence, safety checks, or risks.

6. Keep implementation incremental.
   Prefer behavior-preserving slices, characterization checks, and narrow first moves. Do not turn a refactoring diagnosis into a big rewrite.

7. Respect user intent about planning versus implementation.
   If the user asked for a review, diagnosis, or plan, stop at the plan. If the user asked for implementation, use the selected lens to shape the first safe move, then implement in small behavior-preserving steps.

   If implementation also includes a requested behavior change, first isolate behavior-preserving refactoring from the requested behavior change, and do not expand refactoring beyond what that change needs.

## Output Contract

Use the output contract from the primary reference.

Also include, near the top:

```text
Selected Lens
Why This Lens
Secondary Lens, if any
```

Scale the output to the request size:

- small request or partial context: use at most `Selected Lens`, `Why This Lens`, `Key Risk`, and `Next Safe Steps`
- medium request: use a shortened form of the primary reference with only the sections that drive the first safe move
- high-risk or high-blast-radius request: use the full output contract from the primary reference

If evidence is partial, explicitly label the output as provisional and include `Assumptions` or `Needed Evidence`.

If the request is not a good fit, return:

```text
Not a Fit
Suggested Fallback
```

## Safety Checks

- Do not start by splitting files, extracting layers, or changing shared contracts.
- Preserve current behavior unless the user explicitly asks to change it.
- Prefer characterization tests or manual matrices before invasive movement.
- Keep public APIs, persisted data, events, routes, and external contracts stable unless compatibility is explicitly addressed.
- Treat missing information as an assumption, not as proof.
- After each planned step, name the smallest meaningful verification.

## Common Traps

- Applying all three lenses and producing a generic architecture essay.
- Choosing by file type instead of by dominant risk.
- Triggering on vague words like "cleanup", "messy", or "hard to change" when no structural refactoring decision is actually needed.
- Treating UI refactoring as automatically state-space work; many UI problems are responsibility-boundary problems.
- Treating service refactoring as automatically responsibility work; shared service changes may be dependency-impact problems.
- Moving code before identifying the behavior or contract being preserved.
- Creating abstractions that hide the same complexity under more impressive names.

## Example Routing

- "This form has edit/view/history modes, permissions, async save, and weird disabled buttons." Use state-space primary; maybe responsibility secondary if ownership of action rules matters.
- "This service validates, authorizes, maps DTOs, calls a vendor, writes persistence, and decides policy." Use responsibility-boundary primary.
- "Changing this shared DTO breaks admin, billing, API clients, and vendor sync." Use dependency-impact primary.

## Self-Check

Before responding, verify:

- one primary lens is explicit
- no more than one secondary lens was used
- selected reference files were actually needed
- the plan preserves behavior before restructuring
- the output is a practical refactoring plan, not a broad design review
