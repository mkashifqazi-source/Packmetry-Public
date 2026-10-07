# Testing and Validation

[Back to overview](../README.md)

## Validation philosophy

Tests treat solver output as a claim that must survive independent invariant checking. Representative fixtures establish known geometry, quantity, orientation and inventory cases; pipeline tests challenge selection with invalid or incomplete candidates. This is more informative than asserting only that a solver returned a success label.

| Area | Representative implemented coverage |
| --- | --- |
| Domain and units | Positive/finite values, quantities, canonical conversions and rotation policies |
| Geometry and verification | Containment, overlap/touching boundaries, references, repeated-instance conservation, inventory and known-weight limits |
| Baseline search | Feasible/impossible fixture expectations, stable ordering, candidate diversity and exact deduplication |
| Canonical construction | Recomputed status/metrics, invalid-report rejection, input non-mutation and result validation |
| Selection | Invalid candidate exclusion, coverage precedence, ordered objectives, stable ties, unknown-data and unsupported outcomes |
| DIM semantics | Divisor units, external measurements, unknown propagation, metric/ranking integration |
| Workflows | Purchase suggestions, Have Boxes inventory impact, Hybrid remainder mapping and separate plans |
| Persistence | Mocked IndexedDB/localStorage, failure handling, history behavior and validated Business JSON round trips |
| Presentation | Server-rendered summaries/legends, pure visualization mapping and source-level integration assertions |

## Fresh audit execution

A disposable validation copy of the pinned main-branch snapshot passed the complete Vitest suite, Astro typecheck and static build. Dependencies were installed from the npm lockfile. The original evidence snapshot and remote source were not modified.

The test environment is Node, not a real browser. Persistence tests use stand-ins, and some UI integration tests inspect source structure rather than drive interactive controls. No Playwright/browser E2E framework or real GPU interaction suite is present. These distinctions limit claims about end-to-end browser behavior.

## CI and build gates

The current GitHub Actions workflow installs locked dependencies and runs typecheck, tests and build on main/PR validation. A successful run for the inspected commit was also observed. Strict Astro/TypeScript checks and static route generation are real gates; there is no configured lint command or deployment pipeline in the inspected workflow.

The build passes with a large-client-chunk warning. No claim is made about load budgets, measured throughput, exhaustive property-based exploration, commercial packing optimality or full accessibility certification.

## What remains to validate

Independent physical packing trials, larger realistic workloads, interactive browser/device behavior, storage differences across browsers and rendering performance remain outside this audit. Modeled fragile/padding/spacing/stacking constraints require implementation and tests before they can be presented as active protections.

Validated here means the inspected software passed its current automated checks and the documentation's local link/diagram/safety checks. It does not mean every packing result is physically usable under constraints the software does not model or enforce.
