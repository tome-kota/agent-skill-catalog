# Architecture / Boundary Review

Purpose: detect structural design problems, responsibility confusion, boundary violations, and domain modeling erosion.

Review only architecture, boundary, ownership, and domain-model placement concerns.

Flag:

- vague responsibility assignment
- unexplained Service, Manager, or Coordinator introduction
- Service pattern overuse
- domain logic moved into procedural services without justification
- aggregate invariant leakage
- encapsulation bypass
- layer boundary violations
- dependency direction violations
- public API or schema changes without impact analysis
- behavior placed in the wrong module, layer, aggregate, or use case

AI anti-patterns:

- introduces "responsibility" without mapping it to domain concepts, invariants, or ownership boundaries
- creates new Service, Manager, or Coordinator objects without proving why behavior cannot live in an existing domain object, aggregate, module, or use case
- moves constraints out of the aggregate or domain object that must own the invariant
- exposes internal state so external code can enforce rules that should be encapsulated
- uses a Service pattern to bypass encapsulation rather than clarify orchestration

Do not flag:

- service or use-case objects that only coordinate external effects
- boundary changes explicitly required by the task
- adapter layers that isolate external dependencies correctly

Return findings using [../constructs/finding-format.md](../constructs/finding-format.md).
