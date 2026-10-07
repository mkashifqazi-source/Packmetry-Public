# Objectives and Alternatives

[Back to overview](../README.md)

## Eligibility before attractiveness

Selection operates on independently verified candidates. It first keeps candidates with the greatest placed-item count, then the smallest unplaced count. Only this best-coverage group participates in objective comparison. A cheaper or smaller candidate cannot win by leaving more requested units unpacked.

Ranking uses lexicographic comparisons, not one weighted scalar combining unlike units. Remaining equal comparisons use original candidate index for a stable tie-break. The recommendation is the best comparable evaluated option, not a global optimum.

## Current comparison rules

| Objective | Ordered priorities after coverage | Exposure |
| --- | --- | --- |
| Balanced | Fewer cartons, less empty volume, greater utilization | Business UI; Personal default |
| Fewest Boxes | Same comparison as Balanced today | Business UI |
| Least Empty Space | Less empty volume, smaller total internal carton volume, fewer cartons, greater utilization | Business UI |
| Lowest DIM Weight | Smaller summed dimensional weight, fewer cartons, less empty volume | Business UI with explicit DIM settings |
| Easier to Carry | Lower maximum carton gross weight, lower total gross weight, fewer cartons, less empty volume | Core implemented/tested; not exposed as a Business choice |
| Lowest Carton Cost | Lower total carton cost, fewer cartons, less empty volume | Core implemented/tested; not exposed as a Business choice |
| Existing Inventory First | Unsupported as a ranking objective | Modeled; Hybrid implements separate staged workflow behavior |

Gross-weight, cost and DIM objectives require complete relevant metrics across the comparable candidates. Missing values produce an explicit insufficient-data result; candidates with unknown metrics are not silently treated as free, weightless or removed so another candidate can win. Unsupported objectives produce an explicit unsupported result.

Balanced does not currently express a configurable compromise across cost, weight and volume. Lowest Carton Cost is a carton-cost comparison without currency normalization or shipping prices. Lowest DIM Weight is a physical measurement comparison, not a rate quote.

## Alternative semantics

After ranking, the pipeline constructs the recommendation and remaining eligible alternatives as separate canonical PackingPlans. The built-in solver provides at most three candidates before deduplication, so a planning call can normally yield at most two alternatives. Many inputs produce none; different search candidates need not imply materially different business outcomes.

Alternative displays consume canonical plan metrics and can select a different canonical plan for downstream summaries and visualization. The selector does not generate alternatives or rerun a solver. Lower-coverage candidates and invalid candidates are not presented as equally eligible recommendations.

## Source/documentation divergence

Earlier ADR-008 text described a single-candidate baseline and unsupported DIM semantics. Accepted ADR-011, ADR-012 and current code supersede those historical limitations: candidate diversity and DIM selection are implemented. Existing-inventory-first ranking remains unsupported.

One alternative-explanation branch still says DIM semantics are required before an optimized comparison, despite current DIM implementation. That UI wording is stale; this showcase describes the ranking code and tests rather than presenting the wording as current engine behavior. The source was not modified during this audit.
