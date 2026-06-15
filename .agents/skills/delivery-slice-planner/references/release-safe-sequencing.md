# Release-Safe Sequencing

## Purpose

Design change order from production rollout, compatibility, observability, and rollback safety rather than implementation completion order.

Release safety is not an after-the-fact check. It affects how slices should be cut.

## When To Use

- DB schemas, data migration, API contracts, external integrations, authorization, batches, or UI enablement are involved.
- Old code and new code will run at the same time for some period.
- Staged rollout, feature flags, configuration switches, or rollback are needed.
- Post-release observability is needed to judge success, abnormal states, or support inquiries.

## States To Check

- `Old state`: The system works with the pre-change code, data, and contracts.
- `Dual-support state`: Both old and new behavior work.
- `New state`: The system works with new code, data, and contracts.
- `In-migration state`: Some parts are migrated or enabled while others are not.
- `Rollback state`: Consistency remains intact after rollback.

## Common Sequences

### DB Changes

1. Add backward-compatible columns or tables.
2. Add dual old/new reads and writes.
3. Migrate or backfill data.
4. Switch reads to the new path.
5. After stabilization, delete old columns, old paths, or old processing.

### API Changes

1. Prepare APIs or responses that support both old and new contracts.
2. Move clients or consumers in stages.
3. Observe usage.
4. Decide the stop date for the old contract.
5. Delete the old contract.

### Feature Flags

1. Make code deployable to production while disabled.
2. Enable for internal users or limited conditions.
3. Observe logs, metrics, and support impact.
4. Expand in stages.
5. After stabilization, clean up the flag and old path.

### Authorization Changes

1. Confirm the interpretation of existing permissions and audit logs.
2. Add the new decision logic disabled or impact-limited.
3. Observe affected users and operations.
4. Enable the change.
5. Prepare handling procedures for exceptions or incorrect decisions.

## Rollback Checks

- Is reverting code enough?
- Does data also need to be reverted?
- Would reverting data break user operation history or external integration results?
- Can old code read new data without breaking?
- Can events or notifications sent to external systems be reversed?
- Can the operation be rerun after rollback?

## Observability Checks

- What indicates success?
- What indicates abnormal behavior?
- How is the impact scope narrowed?
- When a support inquiry arrives, which IDs, logs, screens, or data can be used for investigation?
- Is an alert needed, or are manual checks sufficient?

## Bad Release Order

- Schema changes, code changes, data migration, UI enablement, and old-path deletion ship together.
- A migration that cannot be safely rolled back is deployed to production before verification.
- The plan treats the existence of a feature flag as sufficient for safety.
- There are no logs or metrics, so success and failure cannot be distinguished.
- Old clients, batches, admin screens, and external integrations have not been checked.

## Output Integration

Include the following in `Release And Rollback Notes`:

- Recommended release order
- Whether feature flags or configuration switches are needed
- Old/new coexistence period
- Production operations that require procedures, execution approval, execution records, or monitoring checks
- Rollback possibility and constraints
- Post-release confirmation method
- Timing for old-path deletion
