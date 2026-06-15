# Signals Of Core Decision Logic

Treat a change as likely business logic, core decision logic, or system-critical control logic when one or more of the following are true:

- branching changes who gets what outcome
- thresholds, caps, pricing, eligibility, ranking, scoring, or allocation rules change
- state transitions or lifecycle handling change
- validation or exception rules change
- rules for inventory, reservation, settlement, approval, publishing, aggregation, or reconciliation change
- permission, visibility, or approval decisions change
- the meaning of idempotency, deduplication, ordering, or retry changes
- externally meaningful value conversion or rounding changes
- privilege-escalation paths, authorization-cache invalidation boundaries, or session boundaries change
- queue order, backpressure handling, replay conditions, or failover decisions change
- feature-flag targeting semantics or application conditions change
- audit-event, trace, or alert emission conditions change or disappear
- control decisions for delivery, job execution, or consistency preservation change

Signals with especially high human-review value:

- new `if`, `switch`, guard, or special-case branches
- changed handling per enum or status
- changed default-case or fallback behavior
- changed timeout, retry, rollback, or compensation handling
- changed authorization paths, failover paths, or queue admission conditions
- changed feature-flag conditions, audit-output conditions, or cache invalidation rules
- removed or loosened constraints
- added special handling per customer type, plan, region, or role

Signals with relatively low manual-review value:

- import-only cleanup
- formatting-only changes
- generated-file refresh only
- renames with behavior preserved by tests
- shallow wiring with no added semantic branching
