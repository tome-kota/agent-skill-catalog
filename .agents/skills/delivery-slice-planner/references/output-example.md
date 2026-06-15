# Output Example

## Example: API Change With A DB Column Addition

Assumptions:

- Add `display_name` to an existing API.
- Existing clients must not be broken.
- Add a new column to the DB.
- UI exposure will happen later.

```md
# Delivery Plan

## Delivery Target

Add a display name to the existing API so a later UI can use it. Preserve backward compatibility for existing clients, and roll out the DB change and API exposure in stages.

- Plan confidence: Medium. The list of existing clients and the allowed production backfill duration are still unconfirmed, but the DB/API/UI splitting direction can be judged.

## Slicing Strategy

- Primary strategy: Compatibility layer first
- Supporting strategies: Risk-first, observability first

## Proposed Slices

| Slice | Deliverable/PR | Deploy/Enablement | Purpose | Included changes | Excluded changes | Dependencies | Review focus | Verification | Done condition | Release notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1. Backward-compatible DB addition | PR1 | Deployable/no feature impact | Prepare a place to store the new value where old and new code can coexist | Nullable column addition, migration, schema tests | API response changes, UI changes, old-path deletion | None | Whether old code works with the new schema | Migration dry run, existing regression tests | Integration branch or release target remains deployable, and old code can ignore the new column | Old code must keep working even if the DB is not rolled back |
| 2. Dual-support writes | PR2 | Deployable/no enablement | Write the new value without affecting existing users | Save logic, unit tests, logs | API exposure, UI exposure | PR1 | Handling of unset values and impact on existing update flows | Unit tests, integration tests | Existing screens/APIs keep working while old and new data coexist | Confirm logs for write failures |
| 3. API response addition | PR3 | Deployable/old-client compatible | Expose the new field in a backward-compatible way | Response field addition, contract tests | Making the field required, deleting old fields | PR2 | Whether this is an additive change that does not break existing clients | Contract tests, representative client checks | Representative old-client paths succeed | Observe usage through logs |
| 4. Backfill | PR4 + production operation procedure + execution approval + execution record | Run job in stages | Make existing data usable by the new field | Backfill job, rerun design, count logs, operation procedure | UI exposure, old-path deletion | PR2 | Idempotency, rerun after failure, load, operation approval | Staging run, staged production run, count reconciliation | Execution result is recorded, and failed items can be rerun | Rollback is absorbed on the read side rather than by deleting data |
| 5. UI enablement | PR5 | Staged enablement with a feature flag | Show display names to users | UI display, display conditions, representative E2E path | Old-field deletion | PR3, backfill execution record | Unset-value display, authorization, display conditions | E2E, manual check | Impact can be stopped by disabling the flag, and representative paths succeed when enabled | Check logs after limited enablement |

## Review Plan

- PR1: Review DB backward compatibility closely.
- PR2: Review write rules and unset-value handling closely.
- PR3: Review the API contract and existing-client compatibility closely.
- PR4 and production operation procedure: Review idempotency, rerun behavior, load, execution approval, and execution records closely.
- PR5: Review display conditions, authorization, and user impact closely.

## Test Plan By Slice

- PR1: Migration dry run and existing regression tests.
- PR2: Unit tests for save rules and DB integration tests.
- PR3: API contract tests and representative existing-client checks.
- PR4 and production operation procedure: Staged execution, rerun checks, count-log checks, and execution-record checks.
- PR5: Representative E2E path, manual checks, and feature-flag switching checks.

## Release And Rollback Notes

- Make the DB addition backward-compatible.
- Do not break existing responses when adding the API field.
- Enable the UI in stages with a feature flag.
- Run the backfill in stages only after preparing the production operation procedure, execution approval, execution record, and monitoring checks.
- After backfill, absorb rollback by disabling read/display behavior rather than reverting data.
- Plan old-path deletion separately after usage is confirmed.

## Tech Lead Confirmation Points

- Is it acceptable to treat the API field addition as backward-compatible?
- For backfill failures, is it acceptable to recover by rerun rather than reverting data?
- Is it acceptable to keep old-path deletion out of this scope?

## Unresolved Issues

- Existing client list.
- Allowed production backfill duration.
- Target user scope for UI enablement.
```
