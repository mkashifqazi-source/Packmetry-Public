# Dimensional Weight and Known-Weight Semantics

[Back to overview](../README.md)

## Two dimension boundaries

Packing containment and utilization use internal carton dimensions. DIM calculations use supplied external carton dimensions. External measurements are not silently replaced by internal measurements when missing. The domain validates positive dimensions, but does not enforce a physical relation between external and internal measurements; input accuracy remains the user's responsibility.

The divisor is explicit: a positive finite value, a length unit and a mass unit. The calculation converts external dimensions into the divisor's length unit, divides their volume by the supplied value, then normalizes the resulting mass to grams. A naked divisor number without units is insufficient to define the calculation.

There is no implemented verified carrier-preset catalog. Documentation or type comments describing future presets do not establish current support. No carrier-specific live numeric assumptions are published in this case study.

## Weight meanings

| Measure | Meaning and unknown-data behavior |
| --- | --- |
| Contents mass | Sum of placed item-instance weights; unknown if any relevant weight is missing |
| Actual gross mass | Contents plus empty-carton mass; unknown if either contribution is unknown |
| Dimensional mass | External volume under the supplied divisor convention; unknown without external dimensions/settings |
| Estimated chargeable mass | Maximum of actual gross and dimensional mass; available only when both are known |

Plan totals remain unknown if a required used-carton contribution is unknown. A missing value is not zero and partial known totals are not presented as complete values. Gross carton limits apply to physical gross mass, not DIM mass.

## Objective integration

Lowest DIM Weight compares summed dimensional mass within the best-coverage verified candidate group. Divisor settings and external dimensions for all comparable used cartons are required; otherwise selection reports insufficient data. It does not automatically substitute a carrier preset or optimize shipping rates.

Business exposes explicit divisor inputs and external carton dimensions in Have Boxes. Its project-backup format preserves those settings. Need Boxes derives internal carton sizes without inventing external dimensions, tare or divisor context; the Hybrid wrapper does not currently expose a general DIM-optimization path.

## Limits of the estimate

The implementation does not incorporate carrier rounding, service-specific minima, negotiated tariffs, live shipping APIs, labels, customs or fulfillment pricing. Estimated chargeable mass is a measurement result under user-supplied assumptions, not a verified billable quote.

Tests cover unit conversion, divisor validation, missing-data propagation, canonical metric calculation, objective selection and backup round trips. These validate software semantics; they do not independently certify a carrier's current commercial rules.
