# Technical Decision Summaries

[Back to overview](../README.md)

These are newly written public summaries of accepted development-repository ADRs and current code, not invented historical ADRs. Earlier limitations are identified where later decisions or implementation superseded them.

## Canonical measurements — ADR-001

**Context:** Mixed input units can make geometry and mass comparisons inconsistent.

**Decision:** Normalize core length to millimeters and mass to grams; convert at UI/import boundaries.

**Why:** One measurement convention makes solver, verifier and metrics comparable.

**Alternatives considered:** ADR-001 records the canonical choice but no separate historical alternatives analysis. No unrecorded evaluation is attributed to it.

**Consequences:** UI display can change independently of stored canonical values. Floating-point arithmetic and input accuracy still require care.

**Current status:** Implemented and tested. This is not exact decimal arithmetic.

## Solver claims separated from verification — ADR-003, ADR-004, ADR-006

**Context:** Solver implementations may change and can produce invalid placements.

**Decision:** Use an asynchronous adapter returning candidate claims, then independently verify every candidate before downstream authority.

**Why:** Checking invariants separately avoids making the search implementation its own judge.

**Alternatives considered:** ADR-003 rejects a synchronous-only contract, callback progress and embedded duplicated carton data; cancellation is deferred. Solver-owned authority is excluded by the accepted verification contract.

**Consequences:** New adapters must preserve input/reference contracts and verification eligibility. Shared domain utilities do not constitute process isolation.

**Current status:** Implemented and tested in one program. Async shape is present; Worker execution and cancellation are not.

## Canonical construction after selection — ADR-007, ADR-009

**Context:** Summary, export and visualization require a consistent result without inventing separate metrics or geometry.

**Decision:** Select an independently valid candidate, then construct its PackingPlan with copied placements and freshly derived status/metrics; construct eligible alternatives through the same path.

**Why:** Downstream consumers receive one result contract instead of interpreting raw solver claims independently.

**Alternatives considered:** The accepted decisions separate verification, selection and construction rather than merging them or making the solver's claimed status/metrics authoritative.

**Consequences:** Canonical plans can truthfully represent incomplete packing. Consumers must preserve the plan contract; it is not immutable or signed.

**Current status:** Implemented and tested. A populated canonical explanation array is not yet implemented.

## Coverage before ordered objective ranking — ADR-008

**Context:** Optimizing cost or volume first could reward leaving more requested items unpacked.

**Decision:** Exclude invalid candidates, preserve best coverage, then use objective-specific lexicographic comparisons and stable ties.

**Why:** Feasibility and completeness precede attractiveness; incompatible units need no arbitrary scalar weights.

**Alternatives considered:** The ADR rejects a normalized scalar score, solver-selected winners, invalid-candidate ranking, objective-before-coverage and treating missing cost/weight as zero.

**Consequences:** Unknown required data and unsupported objectives are explicit outcomes. Balanced is currently the same baseline comparison as Fewest Boxes.

**Current status:** Implemented and tested. Historical single-candidate and unsupported-DIM limitations in ADR-008 are superseded; existing-inventory-first ranking remains unsupported.

## Bounded deterministic alternatives — ADR-011, revising ADR-005

**Context:** A single candidate makes alternative selection structurally possible but rarely useful.

**Decision:** Run the same greedy heuristic with three carton-order strategies and remove exact duplicates.

**Why:** Small deterministic diversity expands evaluated options without factorial search.

**Alternatives considered:** The ADR rejects staying single-candidate, random orders, all permutations, objective-specific generation and forced duplicate alternatives. Item reordering is deferred.

**Consequences:** One to three candidates per call; no guarantee of a distinct alternative or global optimality. Elapsed time is outside candidate determinism.

**Current status:** Implemented and tested; the earlier single-candidate solver description is historical.

## Distinct box workflow orchestration — ADR-010

**Context:** Existing inventory, purchase suggestions and a combination of both answer different user questions.

**Decision:** Compose shared verified planning into Need Boxes, Have Boxes and Hybrid workflows.

**Why:** Purchase suggestions and existing-inventory impact should remain distinguishable instead of fabricating one unchecked plan.

**Alternatives considered:** The ADR rejects three separate solvers, implicit purchase fallback, immediate Hybrid plan merging, volume-only recommendations and unverified common-size claims.

**Consequences:** Hybrid retains separate existing, supplemental and replacement results. Staged inventory use is not a general existing-inventory-first ranking implementation.

**Current status:** Core and Personal implementations tested; Business currently exposes Have Boxes only.

## Explicit DIM and unknown semantics — ADR-012

**Context:** Internal volume, actual gross mass and DIM-based estimates represent different measurements.

**Decision:** Use external dimensions and explicit divisor units; derive chargeable mass only when both actual and dimensional mass are known.

**Why:** Missing measurements must not become invented zero values or carrier assumptions.

**Alternatives considered:** The ADR rejects internal-dimension fallback, bare unitless divisors, hardcoded carrier divisors, contents-only chargeable mass when tare is unknown and missing DIM data treated as zero.

**Consequences:** DIM ranking needs complete relevant measurements. No live shipping-rate authority follows from the calculation.

**Current status:** Implemented and tested in the core and Business Have Boxes path; carrier-preset provenance/catalog capabilities remain future scope.
