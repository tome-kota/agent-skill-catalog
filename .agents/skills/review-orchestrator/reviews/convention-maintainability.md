# Convention / Maintainability Review

Purpose: detect consistency, readability, naming, dead code, duplication, and maintainability problems.

Review only project convention, readability, naming, local consistency, duplication, dead code, and long-term maintenance friction.

Flag:

- naming drift
- inconsistent style with nearby code
- duplicate helper logic
- unused code
- dead branches
- unnecessary abstraction
- over-configurability
- unrelated cleanup mixed into the change
- comments that restate code
- TODO or temporary comments without owner or removal condition
- new patterns inconsistent with existing project conventions

Do not flag:

- intentional convention changes explicitly scoped to the task
- local duplication that avoids premature abstraction
- comments explaining non-obvious business or technical constraints

Return findings using [../constructs/finding-format.md](../constructs/finding-format.md).
