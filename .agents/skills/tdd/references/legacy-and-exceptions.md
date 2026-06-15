# Legacy And Exceptions

TDD needs to stay usable even in legacy work. When the normal TDD chain cannot be started cleanly, treat that as an explicit mode rather than a vague excuse.

## Characterization Mode

Use this before changing existing untested code, to understand and pin the current behavior. This is not a replacement for the normal TDD cycle. It is an explicit pre-step before beginning the normal TDD cycle.

Procedure:

1. Observe the existing behavior.
2. Write a characterization test.
3. Confirm that the characterization test passes.
4. Write a failing test for the intended behavior change.
5. Implement the minimal change.
6. Confirm that existing behavior still holds.

A characterization test records current behavior, but it does not waive the need for an observed failing behavior test for the new change.

## Spike Mode

Use this when design, API behavior, or library behavior is unknown and exploration is required.

Rules:

- The output of a spike is knowledge, not production code.
- Do not mix spike code directly into production code.
- After the spike, reimplement through a TDD cycle.

Record what the spike taught you and which production behavior should be tested first.

## Mechanical Refactoring Mode

Use this for behavior-preserving mechanical changes such as renames, moves, formatting, or type-only changes.

Rules:

- Green evidence is required before the change.
- Behavior must not change.
- Confirm tests after the change.

If a behavior change becomes necessary, stop mechanical refactoring mode and resume the TDD cycle.

## Configuration Or Documentation Mode

Use this for documentation-only or configuration-only changes.

Rules:

- It is valid only when production behavior is untouched.
- If configuration changes runtime behavior, treat that as production behavior and apply a TDD cycle, or explicitly record an exception.

## Emergency Mode

Use this when an urgent production fix is required before the normal cycle can be completed.

Rules:

- State that it is an exception.
- State the risk.
- Add a follow-up testing plan.

Required exception log:

```text
TDD exception:
Reason:
Risk:
Follow-up testing plan:
Deadline:
Tracking destination:
```

Emergency mode is not a shortcut around normal engineering pressure. It is explicit risk acceptance.
