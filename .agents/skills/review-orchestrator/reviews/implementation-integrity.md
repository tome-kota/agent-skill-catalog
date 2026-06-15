# Implementation Integrity Review

Purpose: detect implementation escape hatches that make uncertainty, invalid states, or design mismatch disappear.

Review only implementation integrity, state validity, failure behavior, typing, and compatibility shortcuts.

Flag:

- fallback values that hide invalid states
- empty string, empty array, or default object returns used to mask failure
- ad-hoc literal-specific branches
- catch-and-continue behavior
- silent error handling
- widened types
- `any`
- broad `unknown as X` casts
- optional or nullable expansion
- non-null assertion abuse
- compatibility shims
- new-old schema dual support without explicit requirement
- normalization layers that guess across multiple schemas
- impossible states accepted as valid
- implementation-specific patches that do not represent domain rules

AI anti-patterns:

- fallback values that hide invalid states
- ad-hoc branches for specific inputs
- widened types such as `any`, broad `unknown` casts, optional expansion, or nullable expansion
- compatibility shims for unsupported legacy formats
- normalization layers that guess across multiple schemas
- silent catch-and-continue behavior

Do not flag:

- fallback behavior explicitly required by product specification
- type widening required by third-party boundary handling and immediately narrowed afterward
- compatibility code with documented scope, tests, and removal condition

Return findings using [../constructs/finding-format.md](../constructs/finding-format.md).
