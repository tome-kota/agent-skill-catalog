# State-Space Refactoring

Use this lens when behavior is hard to change because too many states, modes, flags, transitions, and derived values interact. The goal is to observe the state model before changing structure, reduce invalid or duplicated state, and plan safe behavior-preserving refactoring.

This lens is technology-independent. It applies to UI screens, backend workflows, API handlers, batch jobs, stateful services, and ordinary business code.

## Use When

Use this lens when behavior depends on several of these:

- create, edit, view, detail, history, review, or workflow modes
- selected entities, active tabs, filters, route/query state, or persisted settings
- loading, error, success, retry, stale, or optimistic-update state
- validation state, dirty state, submit state, or operation-specific validity
- user role, permission, tenant, account status, or feature flags
- field visibility, field editability, action availability, layout variant, or data perspective
- derived values computed in multiple places
- event handlers that mutate several state values at once
- conditionals that imply unsupported, illegal, or impossible state combinations
- side effects triggered by state changes

The signal is not file size. The signal is that understanding "what can happen" requires reasoning across combinations of state.

## Do Not Use When

Do not use this as the primary lens when:

- the change is a simple presentational extraction, rename, formatting change, or local bug fix
- the main issue is unclear semantic ownership or mixed responsibilities
- the main issue is consumer impact, shared contracts, or migration risk
- the state model is already explicit and the task is only implementation
- the user explicitly asks for a framework tutorial or library-specific state-machine implementation

## Core Principle

Do not start by splitting files mechanically. First model the state space.

A smaller file with the same hidden state combinations is not simpler. Refactoring is useful only if it makes source state, derived state, valid modes, transitions, ownership, rendering, and side effects easier to inspect.

## Workflow

If code is unavailable or partial, treat this lens as a provisional diagnostic checklist; ask for or name the evidence needed instead of filling the sections as facts.

1. Inspect the current behavior surface.
   Read the main implementation, nearby handlers, selectors/computed values, validators, tests, routes, persistence points, and side-effect code. Identify user-visible behavior or externally observable behavior before proposing changes.

2. Inventory observable source states.
   List the state values that are stored, received, persisted, read from the URL, loaded from the server, supplied by permissions, or controlled by the environment. Include implicit states such as "no selection", "stale data", "validation not yet run", or "operation in progress".

   For screens, forms, and workflow UIs, commonly inspect:
   - screen purpose: create, edit, detail, review, alternate read model, snapshot, comparison
   - entity status: draft, submitted, approved, rejected, archived, locked
   - user role or permission: owner, reviewer, admin, privileged viewer
   - operation or action: save draft, submit, approve, reject, cancel, reopen
   - validation profile: draft save, submit, approve, reject, import, publish
   - field or section visibility, field editability, layout variant, and data perspective

   Express the state space compactly when useful:

   ```text
   screen purpose x entity status x user role x action x validation profile x visibility x editability x layout variant
   ```

3. Separate source state from derived state.
   Mark values that can be computed from other values. Look for duplicated booleans, cached derivations, and fields synchronized by handlers or effects. Do not delete duplicated state until you understand why it exists.

4. Identify modes and mutually exclusive states.
   Find states that represent modes: `create/edit/view`, `draft/submitted/approved`, `loading/error/ready`, `manual/automatic`, or similar. Check whether multiple booleans are really one exclusive mode.

5. Map transitions and triggers.
   For each state change, identify the trigger: user action, route change, server response, timer, job step, permission update, validation result, retry, or side effect. Note handlers that change multiple state values together.

6. Identify invalid or impossible combinations.
   Record combinations that should never happen, are ambiguous, are unsupported, or currently rely on scattered guards. Distinguish true illegal states from rare but valid states.

7. Map state to behavior.
   Connect state combinations to rendering branches, enabled actions, validation rules, API calls, persistence writes, emitted events, retries, notifications, navigation, and cleanup behavior. For UI-heavy cases, compare visibility, disabled state, handler guards, validation profile, and post-action effects.

8. Propose a smaller state model.
   Suggest removing duplicated derived state, replacing related booleans with an explicit mode where justified, tightening ownership, making transition functions explicit, or isolating side effects. Use a state machine only when the transition model is central and the added machinery pays for itself.

9. Refactor incrementally.
   Start with characterization tests, a state matrix, or focused assertions for important combinations. Then make one behavior-preserving change at a time.

10. Preserve semantics.
   Do not change UX, API behavior, retry behavior, validation timing, or persistence semantics unless the user explicitly requested it.

## Output Contract

```text
State Inventory
Derived State / Duplicated State
Mode and Transition Map
Invalid or Impossible States
Refactoring Strategy
Safe Step-by-Step Plan
Risks and Characterization Checks
```

Keep each section grounded in evidence from the code. If evidence is incomplete, state the assumption and confidence level.

## Safety Checks

- Verify current behavior before refactoring with tests, snapshots, logs, a manual matrix, or existing fixtures.
- Preserve externally visible behavior unless explicitly changing it.
- Check whether duplicated state exists for latency, optimistic updates, undo/history, persistence, or compatibility reasons.
- Confirm transition ordering when async responses, retries, or cancellation can race.
- Check that invalid states are prevented at the source, not merely hidden at rendering time.
- For workflow screens, verify important role/status/action combinations with a small matrix before moving UI code.
- Treat future variants such as history, audit, comparison, snapshot, embedded, or print views as design pressure, not automatic scope for new mode flags.

## Common Traps

- Replacing state complexity with premature abstraction.
- Hiding transitions inside generic helpers that make behavior harder to inspect.
- Introducing a state machine without proving the state model needs one.
- Splitting components, services, or handlers before understanding state ownership.
- Assuming all boolean flags are bad.
- Deleting duplicated state without checking synchronization semantics.
- Treating derived display convenience as source state.
- Collapsing rare but valid states into "impossible" states.
- Adding another `mode`, `variant`, or equivalent flag to an already complex flow without proving it is the right model.
- Treating history, audit, snapshot, comparison, or alternate read models as merely readonly versions of edit/detail mode.

## Example

A workflow has `isEditing`, `isReadonly`, `isSubmitting`, `hasError`, `selectedId`, `draftValue`, and `serverValue`. Several handlers set three or four values together.

First inventory source state: selected record, server value, local draft, submit request status, and permission. Then mark derived state, such as readonly from permission, submit status, and selected record status. A safe plan might characterize edit, cancel, submit success, submit failure, and selection change while dirty before extracting derived readonly logic or replacing related booleans with an explicit mode.
