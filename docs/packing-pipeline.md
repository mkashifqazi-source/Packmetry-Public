# Packing Pipeline and Canonical Results

[Back to overview](../README.md)

## From inputs to one recommendation

~~~mermaid
flowchart TD
    I[Normalized input] --> S[Candidate generation]
    S --> V[Independent verification]
    V --> Q{Valid?}
    Q -- No --> X[Report retained; candidate excluded]
    Q -- Yes --> C[Best placement coverage]
    C --> D{Supported and comparable?}
    D -- No --> F[Explicit not-planned]
    D -- Yes --> R[Objective ranking]
    R --> B[Canonical selected and alternative plans]
~~~

If no valid candidate remains, selection also reports an explicit no-valid-candidate outcome. A structurally valid candidate with unplaced items may still be selected and constructed; result status makes incomplete coverage visible.

## Responsibilities

1. Workflow preparation supplies carton definitions and item subsets appropriate to the requested mode.
2. The solver owns generation of candidate placements. Its output has no authority merely because the built-in solver created it.
3. Integration independently verifies every returned candidate and retains its report and original metadata. It does not rank.
4. Selection filters invalid candidates, establishes the best coverage group, computes comparison metrics and ranks that group. It does not run another solver or repair placements.
5. Canonical construction rejects an invalid report, copies selected content and derives status and metrics from the inputs and placements. The same construction is used for remaining eligible ranked alternatives.

Ranking occurs before canonical plan construction but after verification. Candidate comparison metrics and canonical result metrics use the implemented domain/measurement rules; selection does not depend on preexisting canonical plans.

## What canonical means

One PackingPlan supplies placement truth to downstream views. Summaries and visualization do not independently solve the geometry. Alternatives are separately constructed canonical plans; switching between them changes which plan consumers display.

Canonical construction recomputes volume, utilization, known costs/weights, DIM metrics where configured, and status. It does not accept arbitrary candidate metric claims. The normal pipeline therefore prevents attractive invalid candidates from winning and prevents a misleading solver status from becoming the final status.

The engine's explanation array is currently empty. UI rationale is derived from canonical metrics and the selected objective; a populated machine-readable explanation/evidence subsystem remains unimplemented. Canonical objects are not cryptographically verified or frozen against later mutation.

## Box workflow composition

| Workflow | Current behavior |
| --- | --- |
| Need Boxes | Generate up to three unique carton-size definitions from deterministic row-layout estimates, then run the shared planner. No supplier/catalog matching, purchase price or invented tare is supplied. |
| Have Boxes | Plan using the supplied cartons and bounds; report used, unused and remaining inventory. No purchase fallback or inventory write-back occurs. |
| Hybrid | First plan existing cartons. If that produces a checked incomplete plan, derive the remaining items, plan supplemental cartons, and separately calculate a full-replacement comparison. |

Hybrid preserves item-instance correspondence when deriving the remainder. Existing and supplemental plans remain separate; the workflow does not concatenate placements into an unverified combined plan or automatically declare a financial winner. A failed initial planning outcome does not silently become a purchase recommendation.

Personal exposes all three workflows. Business exposes Have Boxes. Explicit DIM settings are wired through Have Boxes; Need Boxes and Hybrid are not general DIM-optimization workflows today.
