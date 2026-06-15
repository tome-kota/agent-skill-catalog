# Review Target Prioritization

Use this file to decide where human attention should go first.

The purpose is not generic risk scoring. The purpose is to push scarce human review time toward changes that require design judgment in business logic, core decision logic, or system-critical control logic.

## Patterns That Raise Priority

Changes matching the following patterns should usually land in `review deeply` or `review if needed`.

### State And Workflow

- new states or statuses are introduced
- terminal-state or cancellation handling changes
- fail-fast versus retry policy changes
- asynchronous ordering or timing changes

### Contracts And Boundaries

- API field meaning changes
- schema nullability, defaults, uniqueness, or ownership changes
- event payload changes may affect downstream consumers
- permission boundaries or tenant boundaries change

### System-Critical Control Decisions

- authorization-cache invalidation boundaries change
- job replay, redelivery, or resume conditions change
- queue ordering or backpressure handling changes
- failover, degrade, or fallback decision conditions change
- feature-flag targeting semantics change
- audit-event, trace, or alert emission conditions change

### Business Damage Risk

- rules touching money, discounts, billing, credit, or quotas change
- rules touching inventory, reservation, seats, or capacity change
- rules touching publishing, approval, moderation, or visibility change
- changes affect auditability, traceability, or compliance behavior

### AI-Assisted Change Patterns Worth Watching

- the change is broad but tests are weak
- many files changed but the semantic core is poorly explained
- the same conditional logic is duplicated across layers
- helper extraction makes decision ownership harder to see
- abstraction was introduced for what still looks like a one-off case
- authorization, retry, auditing, or consistency decisions are split across multiple places

## Using The Four Buckets

To reduce subjectivity, check each hotspot briefly on these four axes:

- `impact size`: how painful it would be if this is wrong
- `semantic change likelihood`: whether business meaning or system meaning may have changed
- `responsibility-boundary impact`: whether ownership or boundary placement is unstable
- `evidence strength`: whether the concern is supported by concrete diff evidence

Strict scoring is not required, but classification should follow these simple rules and include at least one short classification reason.

- `review deeply`: impact size or semantic change likelihood is high, and evidence strength is sufficient
- `review deeply`: responsibility-boundary impact is high, and evidence strength is sufficient
- `review if needed`: semantic change likelihood or responsibility-boundary impact is present, but impact size or evidence strength is limited
- `safe to skim`: semantic change likelihood is low and responsibility-boundary impact is also low
- `delegate to automation`: semantic change likelihood is absent and the change is mostly mechanical

### `review deeply`

Use this when any of the following is true:

- the design judgment itself is central to the change
- rules or state transitions change
- responsibility placement or decision ownership is unstable
- workaround smell is strong
- the damage if wrong is high

### `review if needed`

Use this when any of the following is true:

- the area is not the main concern, but design implications are present
- orchestration or wiring changes touch responsibility boundaries
- test changes imply intent changes
- refactoring affects how visible the core decision logic is

### `safe to skim`

Use this when any of the following is true:

- the change is mostly mechanical
- wiring is shallow and semantics are unlikely to change
- the change is mostly rename or follow-up adjustment

### `delegate to automation`

Use this when any of the following is true:

- lint, format, or import ordering only
- code generation refresh
- snapshot churn without semantic difference

## How To Write Priority Reasons

Whenever you assign a bucket, try to explain it in this shape:

- `what changed`
- `why human review matters`
- `which bucket it belongs in`
- `priority reason`

Examples:

- `A new past_due state was introduced and may affect permission handling, so this belongs in review deeply.`
- `The authorization-cache invalidation boundary changed, and the possible blast radius for over-permission or over-denial is high, so this belongs in review deeply.`
- `This looks like a controller-to-service move only, but it is worth checking whether decision ownership actually improved, so this belongs in review if needed.`
- `Only GraphQL type regeneration changed, so this belongs in delegate to automation.`

Short priority-reason examples:

- `High impact and likely semantic change.`
- `Semantic change seems limited, but boundary placement is affected.`
