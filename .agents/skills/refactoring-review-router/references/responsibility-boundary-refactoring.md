# Responsibility-Boundary Refactoring

Use this lens when complexity comes from unclear ownership of logic. The goal is not to make layers look tidy. The goal is to identify change reasons and move logic so each unit has a coherent reason to change.

This lens is technology-independent. It applies to frontend components, backend services, controllers, API layers, jobs, validators, helpers, domain logic, and integration code.

## Use When

Use this lens when code shows symptoms such as:

- a UI unit contains rendering, validation, authorization, formatting, API calls, and business decisions
- a service contains persistence details, external API concerns, policy decisions, DTO mapping, and orchestration
- utility, common, shared, helper, hook, composable, or manager modules become context-dependent dumping grounds
- one file or function changes for many unrelated feature requests
- new logic has no obvious home
- policy rules are mixed with transport, persistence, framework, or formatting concerns
- orchestration code makes domain decisions inline
- generic abstractions contain domain-specific exceptions
- screens, forms, controllers, or jobs mix authorization, validation, action availability, workflow transitions, display formatting, and side effects
- future variants such as history, audit, comparison, snapshot, privileged view, embedded mode, or alternate read model would amplify existing boundary confusion

The signal is mixed reasons to change, not just size, duplication, or naming.

## Do Not Use When

Do not use this as the primary lens when:

- the main issue is complex modes, transitions, derived state, or invalid state combinations
- the main issue is hidden consumers, contract migration, or blast radius
- the task is a mechanical rename, formatting pass, or style-only extraction
- responsibilities are already clear and the user only needs a local implementation
- the code is duplicated but the duplication protects separate change reasons

## Core Principle

Ask:

```text
What kind of change should cause this code to change?
Are multiple change reasons mixed together?
Is the current boundary semantic, accidental, or framework-driven?
```

Refactoring succeeds when future changes have a natural home and do not force unrelated concerns to move together.

## Workflow

If code is unavailable or partial, treat this lens as a provisional diagnostic checklist; ask for or name the evidence needed instead of filling the sections as facts.

1. Identify current units.
   List the functions, classes, components, modules, jobs, handlers, validators, helpers, and shared abstractions involved. Include adjacent tests and callers when they reveal intended ownership.

2. Map current responsibilities.
   For each unit, list what it does in concrete terms: render, validate, authorize, format, decide policy, orchestrate workflow, map data, persist data, call external systems, handle retries, translate errors, emit events, or coordinate side effects.

   For screen-like or workflow-like units, separate these responsibilities when present:
   - screen or flow purpose
   - data loading and saving
   - workflow transitions
   - authorization and privileged controls
   - action availability
   - operation-specific validation
   - field or section visibility
   - field editability
   - display formatting
   - layout, route, or application chrome
   - navigation, notifications, telemetry, auditability, or explanation behavior

3. List change reasons by unit.
   Ask which future requests would require this unit to change. Separate product rule changes, policy changes, data-shape changes, framework changes, vendor changes, presentation changes, persistence changes, and operational changes.

4. Separate semantic responsibilities from technical mechanisms.
   A route, hook, controller, job, or service name may reflect framework placement rather than real ownership. Identify the business or operational concept that should own each decision.

5. Detect responsibility leaks.
   Look for domain policy in generic utilities, authorization in rendering, persistence details in domain logic, vendor payloads in core rules, formatting in services, validation duplicated across layers, and orchestration code making hidden policy decisions.

   In UI or workflow code, also look for role/status/action checks embedded directly in rendering, validation tied only to a visual form, action buttons whose visibility and handler guards disagree, duplicated field visibility/editability conditions, and shared components that know too much about screen purpose, permissions, workflow, or layout.

6. Decide move, extract, inline, or leave.
   Move logic when ownership is clear. Extract only when the extracted unit has a coherent reason to change. Inline when an abstraction only hides mixed concerns. Leave duplication when separate change reasons are more important than reuse.

7. Propose target boundaries.
   Name the target owner for each important responsibility. Prefer local patterns already present in the codebase: policy objects, validators, presenters, selectors, use cases, services, mappers, repositories, adapters, modules, or plain functions.

   Common target boundaries include:
   - decision-logic boundary: inspectable authorization, visibility, editability, and action availability rules
   - validation boundary: validation attached to operations and workflow transitions, not just forms
   - workflow/action boundary: transition preconditions, execution, API calls, optimistic updates, navigation, and notifications
   - display boundary: stable readonly display sections, labels, formatters, and simple field renderers
   - layout/context boundary: normal app chrome, embedded, print, chrome-free, audit, snapshot, or alternate read-model contexts

   Match the codebase. These boundaries can be implemented as selectors, policies, presenters, view models, configuration, validators, use cases, controllers, domain services, modules, or plain functions.

8. Plan incremental movement.
   Preserve behavior while moving one responsibility at a time. Start with pure or easily tested logic when possible, then adjust orchestration and callers. Prefer moving decision logic, validation profiles, or action availability before moving large rendering blocks.

9. Verify ownership and behavior.
   Add or preserve tests that prove the moved responsibility still behaves the same. Check that the new boundary reduces future change friction rather than just moving lines.

## Output Contract

```text
Current Responsibility Map
Change Reasons by Unit
Boundary Smells
Target Responsibility Boundaries
Move / Extract / Inline Decisions
Safe Refactoring Sequence
Risks and Verification Points
```

Each boundary recommendation should name the reason for change it protects.

## Safety Checks

- Preserve behavior before and after each move with tests or characterization checks.
- Keep public API, route, schema, event, and persistence contracts stable unless explicitly changing them.
- Verify that extracted logic no longer depends on the old mixed context.
- Check that authorization, validation, and policy decisions remain enforceable outside presentation code.
- Avoid moving code across ownership or team boundaries without identifying the new owner.
- Verify that visible actions, disabled reasons, handler guards, and validation rules still agree after extraction.
- Prefer sharing low-level stable display primitives over sharing page-level abstractions when screen purpose differs.

## Common Traps

- Creating layers only because architecture diagrams look cleaner.
- Extracting functions that preserve the same mixed responsibility.
- Moving logic based only on file size.
- Treating all duplication as bad.
- Putting domain-specific logic into generic utilities.
- Creating `manager`, `handler`, `helper`, or `service` objects without a clear responsibility boundary.
- Mistaking framework boundaries for semantic boundaries.
- Centralizing unrelated rules into a single "rules" object that changes for every reason.
- Hiding business rules inside generic UI components.
- Moving all complexity into one huge hook, store, controller, presenter, use case, or service.
- Extracting components only because markup looks duplicated.
- Treating visual similarity as proof that two use cases share the same boundary.

## Example

An API handler validates input, checks whether a user may approve an order, formats a vendor payload, calls the vendor, updates local persistence, and chooses the response DTO.

Map change reasons: approval policy changes with business rules, vendor payload changes with the provider, persistence changes with storage, response DTO changes with API consumers, and orchestration changes with workflow. A safe plan might extract approval policy into a focused rule owner, extract vendor payload mapping into an adapter, leave orchestration in the handler or use-case layer, and preserve the response contract.
