# Software Coupling Review

## Purpose

Review how tightly modules, services, schemas, events, and teams are tied together, and whether that coupling creates durable leverage or recurring change pain.

## When to use

Use this reference when the main issue is boundary quality, synchronized change, shared schemas, multi-service friction, or uncertainty about whether a split or merge will improve the design.

## Primary questions

- Which dependencies are expensive because they are strong, distant, or unstable?
- Where is semantic coupling hidden behind apparently clean interfaces?
- Which boundaries reduce coordination cost, and which ones merely move it around?
- Is the design concentrating change locally, or forcing change choreography across components?

## Review procedure

1. Map the relevant boundaries.
2. Identify where semantics, runtime behavior, and operations are shared.
3. Separate healthy local coupling from painful distributed coupling.
4. Find the boundary stress points that drive synchronized change.
5. Recommend whether to merge, split, translate, or narrow the contract.

## Output cues

Produce a review that identifies change-friction hotspots, explains why they are painful, and recommends boundary adjustments in terms of coordination cost.

## Review stance

Do not treat all coupling as bad.

Strong coupling can be acceptable when it is:

- local
- conceptually honest
- owned by one team
- stable enough to change together

Coupling becomes expensive when dependency strength, distance, and volatility combine to force repeated coordination.

## Boundary map

Start by locating:

- modules
- services
- databases
- event streams
- APIs
- shared libraries
- pipelines
- team ownership boundaries

The question is not "what connects?" but "what must change together?"

## Coupling lenses

Review coupling through these lenses:

- semantic coupling
- runtime coupling
- development coupling
- operational coupling
- schema and contract coupling
- ownership coupling

Each lens can be mild or severe on its own. The painful cases are the ones that stack.

## Semantic coupling checks

Look for shared concepts that appear identical across contexts but do not mean the same thing.

Signals:

- identical entity names across services
- shared enums with divergent business meaning
- one context forcing another to inherit its vocabulary
- downstream systems carrying attributes they do not truly need

Semantic coupling is often the most dangerous because it survives refactors and hides behind "clean" interfaces.

## Runtime coupling checks

Look for dependencies that force components to behave as one runtime unit.

Signals:

- deep synchronous call chains
- startup ordering constraints
- cascading retries
- distributed transactions
- mandatory orchestration for common flows

Ask:

- can this component fail independently?
- can it degrade independently?
- can it recover without the other side?

## Development coupling checks

Look for dependencies that force code change and release coordination.

Signals:

- simultaneous PRs across repositories
- lockstep schema changes
- shared DTO churn
- multi-team release sequencing
- shared CI or version upgrade bottlenecks

If one small domain change routinely needs several teams, the boundary is probably underperforming.

## Operational coupling checks

Look for dependencies that tie scaling, availability, or operability together.

Signals:

- shared databases
- shared queues or caches
- shared failure domains
- assumptions that every service has the same observability
- one component's load profile dictating another's behavior

Operational coupling matters even when code ownership looks clean.

## Boundary stress patterns

### Shared storage as hidden coordination

The design appears decoupled in code, but coordination happens through tables, columns, caches, or files.

### Contract drift without translation

Multiple contexts rely on the same shape while needing different meanings over time.

### Distance without independence

The system is physically split, but still changes as if it were one module.

### Transport-shaped design

Domain boundaries are being chosen to fit an API, DTO, or broker contract rather than the actual concepts.

### Local split, global pain

A decomposition reduces local complexity but increases cross-boundary negotiation every time the domain changes.

## What good coupling looks like

Prefer designs where:

- the strongest coupling stays close to the concept that changes
- shared meaning is minimized across distant boundaries
- translation happens at context edges
- failure domains are isolated
- team ownership lines match technical responsibility

## Deep checks

Use this section when the initial review suggests that the boundary story is cleaner than the actual change behavior.

### What to probe further

- whether the same business change repeatedly lands in several repos or services
- whether shared terms hide different semantics across contexts
- whether deployment independence is real or only nominal
- whether a proposed split reduces ownership clarity while claiming architectural purity

### Strong warning signs

- the same concept requires synchronized PRs in several places
- contracts are stable syntactically but unstable semantically
- translation is avoided because teams want "one shared model"
- a design calls itself decoupled while operations still fail as one unit

### Review prompts

- What must change together when this domain rule changes?
- Which dependency is cheapest locally but most expensive at distance?
- What boundary exists only on paper?
- Would merging these responsibilities reduce total coordination cost?

## Recommendation rules

- Merge components when they always change together for the same business reason.
- Split components when shared deployment or ownership is masking truly different change pressures.
- Add translation layers when the same words are carrying different meanings across contexts.
- Narrow contracts when downstream consumers are inheriting upstream detail they do not need.
- Prefer local complexity over distributed coordination when the same team owns both sides.

## Output shape

Structure findings around:

- key boundary stress points
- the type of coupling involved
- why the current shape creates coordination cost
- what change becomes easier after the recommendation

## Acceptance checks

A strong review from this reference should:

- identify where change is forced to synchronize
- distinguish semantic, runtime, development, and operational coupling
- avoid treating all coupling as equally harmful
- explain whether the current boundary is helping or hurting
- recommend a concrete boundary move, not just "reduce coupling"
