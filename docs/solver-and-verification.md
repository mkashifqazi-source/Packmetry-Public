# Solver and Independent Verification

[Back to overview](../README.md)

## Current solver behavior

The baseline is a deterministic greedy heuristic for rectangular, axis-aligned items. It expands quantities into item instances in input order, tries already opened cartons before opening another, considers permitted orientations, and searches geometric placement opportunities without intersecting previously placed interiors.

Carton selection respects supplied inventory bounds. Known gross-weight limits can prevent a placement when tare and all relevant item weights are available. Items that cannot be placed remain explicit with reason classifications; a failure to find a placement is not a mathematical proof that no arrangement exists.

Candidate diversity comes from carton-type order: original order, ascending internal volume and descending internal volume. Equal-volume ties preserve input order. Each strategy uses the same placement heuristic and item ordering. Exact serialized candidate duplicates are removed, preserving first occurrence; non-identical plans are not merged into a semantic equivalence class.

This yields one to three candidates per solver invocation. It is not exhaustive permutation search, random search, item-order optimization or a global optimum guarantee. The asynchronous adapter permits replacement implementations, but the built-in solver currently runs on the calling thread. Candidate content and ordering repeat for identical ordered inputs; runtime duration metadata may differ.

## Verifier coverage

| Check | Current enforcement |
| --- | --- |
| References | Every carton and item reference must resolve to the original normalized input |
| Instance accounting | Valid instance indices; every requested unit exactly once across packed and unplaced records |
| Rotation | Allowed policy and oriented dimensions consistent with the input item |
| Boundaries | Nonnegative placement and containment in internal carton dimensions, allowing numeric tolerance |
| Overlap | Pairwise axis-aligned interior intersections rejected; touching faces are permitted |
| Inventory | Opened carton instances cannot exceed the tighter supplied availability/stock bound |
| Gross weight | Checked only when a gross limit, tare and all placed item weights are known |

The verifier checks output independently of the placement search and ignores the solver's feasibility label. It shares domain types and rotation utilities; independence is logical, not a separately deployed service or a formal proof system.

Missing weight data does not cause verification failure. A candidate can be independently valid under implemented checks while physical weight feasibility remains unknown. Fragile/padding/spacing/stacking semantics and load stability are not checked. Candidate metrics are not compared against claimed values because metrics are constructed downstream from the actual placements.

## Authority and failure boundary

Integration pairs all candidates with reports. Selection excludes invalid reports before coverage or objective ranking. Canonical construction also rejects an invalid report and recomputes result status. Tests exercise fabricated invalid geometry, quantity errors, inventory excess and attractive-but-invalid candidate selection.

These guards protect the normal pipeline. Passing a forged report to a low-level construction helper or mutating a result later is outside that orchestration contract; this is not an adversarial authorization boundary.

If no candidate is valid, there is no canonical recommendation. If valid candidates leave items unplaced, the result is explicitly partial or infeasible. Unknown objective metrics yield insufficient-data results rather than zero-valued substitutes. Future solvers must preserve these contracts instead of granting their own output authority.
