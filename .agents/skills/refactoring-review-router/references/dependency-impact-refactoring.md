# Dependency-Impact Refactoring

Use this lens when a change is dangerous because dependency direction, consumers, contracts, and blast radius are unclear. The goal is to reduce change propagation with a staged strategy, not merely draw a dependency graph.

This lens is technology-independent. It applies to frontend shared modules, backend services, schemas, DTOs, APIs, persistence models, jobs, libraries, framework integrations, vendor clients, and tests.

## Use When

Use this lens when:

- changing one type, schema, API, shared module, DTO, event, or helper affects many consumers
- shared code has hidden or poorly understood consumers
- upper layers depend on lower-level implementation details
- domain logic depends directly on transport, persistence, framework, or vendor APIs
- old and new behavior must coexist temporarily
- migration must be staged safely
- tests are tightly coupled to implementation details
- a team wants to reduce blast radius before adding a feature
- compile success is not enough to prove behavioral safety

The signal is change propagation risk: a local edit may break unrelated areas or external consumers.

## Do Not Use When

Do not use this as the primary lens when:

- the change is local and all consumers are obvious
- the main issue is complex states, modes, or transitions
- the main issue is mixed responsibilities or unclear ownership
- the task is purely formatting, renaming, or deleting unused code with no consumers
- the user wants only a conceptual explanation, not a migration or refactoring strategy

## Core Principle

Ask:

```text
Who depends on this?
Why do they depend on it?
Which dependencies are semantic, and which are accidental?
Where can an adapter, compatibility layer, or boundary reduce blast radius?
```

A safe refactor changes dependency pressure before changing the risky shared thing.

## Workflow

If code is unavailable or partial, treat this lens as a provisional diagnostic checklist; ask for or name the evidence needed instead of filling the sections as facts.

1. Identify the change target.
   Name the type, module, schema, endpoint, DTO, event, helper, service, table, job, or behavior being changed. Clarify whether the change is behavioral, structural, naming, data-shape, contract, or dependency-direction related.

2. Trace direct consumers.
   Use search, static references, tests, route declarations, schema references, import graphs, API clients, job configuration, generated code, documentation, and known runtime entry points. Include non-code consumers when visible.

3. Trace indirect consumers.
   Follow wrappers, re-exports, mappers, inheritance, callbacks, events, serialized data, persisted records, queues, caches, feature flags, and test fixtures. Record uncertainty instead of pretending the graph is complete.

4. Classify dependency types.
   For each consumer, classify the dependency:
   - semantic dependency: relies on business meaning or behavior
   - data shape dependency: relies on fields, schema, payload, or serialization
   - control-flow dependency: relies on call timing, ordering, callbacks, retries, or side effects
   - framework dependency: relies on framework lifecycle, annotations, hooks, middleware, or configuration
   - persistence dependency: relies on tables, queries, migrations, indexes, storage format, or transactions
   - API/transport dependency: relies on endpoint shape, HTTP behavior, events, queues, messages, or vendor protocol
   - test dependency: relies on fixtures, mocks, snapshots, factories, or implementation details

5. Assess blast radius.
   Identify which consumers are internal, external, high-value, hard to test, operationally sensitive, versioned, generated, or owned elsewhere. Separate compile-time breakage from behavioral breakage.

6. Identify semantic versus accidental dependencies.
   Preserve semantic dependencies that express real domain needs. Reduce accidental dependencies on field names, transport details, persistence structures, vendor payloads, framework lifecycle, or incidental helper behavior.

7. Find safe boundary points.
   Look for places where an adapter, facade, mapper, compatibility layer, feature flag, versioned endpoint, anti-corruption boundary, wrapper, or temporary bridge can isolate consumers. Add such boundaries only when they support a concrete migration.

8. Design a staged plan.
   Prefer this sequence: characterize current behavior, add compatibility boundary, migrate one consumer group, verify, repeat, then remove obsolete paths after confidence and ownership are clear.

9. Preserve compatibility where needed.
   Keep old and new behavior coexisting when external consumers, persisted data, async jobs, rolling deploys, or cross-service calls require it. State when compatibility can be dropped.

10. Define rollback and verification.
   Include contract tests, characterization tests, migration checks, telemetry, feature flags, canary paths, or manual verification appropriate to the change. Identify the rollback point before the risky edit.

## Output Contract

```text
Change Target
Dependency Map
Consumer Classification
Blast Radius Assessment
Semantic vs Accidental Dependencies
Safe Boundaries / Adapters / Compatibility Points
Staged Refactoring Plan
Rollback and Verification Strategy
```

Mark unknown consumers and assumptions explicitly.

## Safety Checks

- Do not change shared contracts before mapping consumers.
- Preserve compatibility for external callers, persisted data, queued messages, generated clients, and rolling deployments unless explicitly allowed to break them.
- Add contract or characterization checks before risky shared changes.
- Verify both compile-time and runtime behavior where dependency type demands it.
- Confirm that adapters have a migration purpose and an intended removal or stabilization path.
- Keep useful characterization tests until the migration risk has passed.
- Review test fixture changes carefully; they can hide consumer assumptions.

## Common Traps

- Changing shared types before mapping consumers.
- Assuming compile success means behavioral safety.
- Introducing adapters everywhere without a migration purpose.
- Flattening all dependencies into generic interfaces.
- Breaking compatibility before identifying external consumers.
- Refactoring tests in a way that removes useful characterization coverage.
- Treating all dependencies as bad instead of distinguishing semantic from accidental dependencies.
- Migrating every consumer at once when staged coexistence would be safer.

## Example

A shared `CustomerDto` is used by an API response, an admin screen, a billing job, and a vendor sync. A requested change renames `status` and changes how suspended customers are represented.

Map consumers: API clients depend on data shape and compatibility, admin screen depends on display meaning, billing job depends on semantic eligibility, vendor sync depends on transport mapping, and tests depend on fixtures and snapshots. A safe plan might introduce a new internal customer status model, keep the public DTO stable, update the billing job against the semantic model, adapt the vendor payload through a mapper, add contract tests for the public API, then version or migrate the DTO only after external consumers are accounted for.
