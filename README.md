# Packmetry

A browser-first cartonization system that evaluates deterministic packing candidates, independently verifies them, and presents canonical results in summaries and 3D.

Public Technical Overview · Development Repository Maintained Separately

## Overview

Packmetry helps people determine which boxes to use and how rectangular items fit inside them. Personal and Business workspaces share one normalized domain model, solver, verifier and result contract. The current application is a static Astro site with interactive React workspaces; packing computation and browser persistence run locally.

This case study describes the implementation inspected on the development repository's main branch. That repository is publicly visible at audit time; this showcase is a separately curated technical explanation.

## Engineering Problem

Enough aggregate box volume does not establish that items fit. Geometry, permitted orientations, repeated quantities, available inventory and known weight limits all affect feasibility. A favorable score also does not establish that a solver's placements are valid.

Packmetry separates proposing arrangements, verifying invariants, choosing among eligible candidates and presenting the result. Missing information remains unknown rather than becoming zero or an invented shipping estimate.

## Core Architecture

~~~mermaid
flowchart TD
    UI[Personal / Business UI] --> N[Canonical inputs]
    N --> W[Box workflows]
    W --> S[Candidate solver]
    S --> V[Independent verifier]
    V --> R[Coverage and objective ranking]
    R --> C[Canonical PackingPlan]
    C --> T[Summary / alternatives]
    C --> P[History / export]
    C --> G[Three.js renderer]
    UI --> B[Inputs / preferences]
~~~

The solver, verification and planning modules are independent of React, Three.js and browser storage. The asynchronous solver contract is an adapter boundary; the current baseline still computes on the calling thread.

## Packing Pipeline

The baseline runs a greedy axis-aligned placement heuristic with up to three carton-order strategies: supplied order, smaller internal volume first and larger internal volume first. Item order is unchanged. Exact duplicate candidate content is removed. Candidate content and ordering are deterministic for identical ordered inputs; elapsed-time metadata is not.

Every candidate is verified before selection. Selection first preserves the best item coverage, then applies the requested supported objective. The pipeline constructs the selected canonical plan and eligible alternatives with freshly derived metrics and status. A canonical plan may be partial or infeasible; canonical means a consistent checked result, not a promise that every item was packed or that a global optimum was found.

## Independent Verification

The verifier checks item/carton references, instance accounting, permitted rotations, carton boundaries, overlap and supplied inventory limits. Gross-weight limits are checked only when the limit, carton tare and all relevant item weights are known. Missing weights do not invalidate an otherwise geometrically valid candidate.

Invalid candidates cannot become canonical plans through the normal planning pipeline. This is a separate checking implementation within the same program, not an isolated security service. Fragile, padding, spacing and stacking fields exist in the model but are not enforced by the baseline or verifier.

## Optimization and Alternatives

Business exposes Balanced, Fewest Boxes, Least Empty Space and Lowest DIM Weight. Balanced currently uses the same ordered comparison as Fewest Boxes. DIM ranking requires explicit divisor units and external carton dimensions; incomplete comparable metrics produce an explicit insufficient-data result.

The recommendation is the best evaluated, independently valid option under the implemented comparison. Alternatives come from the remaining comparable ranked candidates, not an exhaustive search. See [objectives and alternatives](docs/objectives-and-alternatives.md).

## Box Workflows and 3D Visualization

Personal implements Need Boxes, Have Boxes and Hybrid. Need Boxes derives a small set of carton-size suggestions, not a supplier catalog. Hybrid preserves existing-box and supplemental plans separately and also calculates a replacement comparison. Business currently exposes Have Boxes with SKU and advanced carton inputs.

Three.js draws canonical placements; it does not calculate new packing arrangements. Per-carton selection, orbit controls, item legends and text summaries accompany the view. Browser/GPU compatibility and complete accessibility have not been independently certified.

## Technology and Validation

The verified stack is Astro, React, strict TypeScript, Three.js, npm and Vitest. IndexedDB stores local histories and libraries; localStorage stores preferences. CI runs typecheck, tests and a static build. There is no mandatory account, server-side packing engine or paid API dependency in the inspected application.

A fresh isolated audit run passed the Vitest suite, Astro typecheck and static build. Tests cover domain validation, geometry, quantities, limits, deterministic candidates, verification gates, canonical metrics, objectives, DIM calculations and browser adapters. Component coverage includes server rendering and source-level integration assertions; it is not browser E2E testing.

## Current Status

Implemented and tested browser-first engineering baseline, with heuristic search and explicitly conditional constraints. No global-optimality, shipping-rate or deployed-service guarantee is made. Physical support/fragility semantics, a standalone product catalog, CSV/batch workflows, cloud accounts and integrations remain outside the implemented scope. The build succeeds with a large-client-chunk warning; realistic performance and browser interaction validation remain open. See the [capability/status matrix](docs/project-status.md).

## Technical Documentation

- [Architecture](docs/architecture.md) · [Domain model](docs/domain-model.md)
- [Packing pipeline](docs/packing-pipeline.md) · [Solver and verification](docs/solver-and-verification.md)
- [Objectives and alternatives](docs/objectives-and-alternatives.md) · [Dimensional weight](docs/dimensional-weight.md)
- [Visualization](docs/visualization.md) · [Persistence and privacy](docs/persistence-and-privacy.md)
- [Testing and validation](docs/testing-and-validation.md) · [Technical decisions](docs/technical-decisions.md)
- [Project status](docs/project-status.md)

## Repository Notice

This repository contains curated public technical documentation for Packmetry. Development source and project materials are maintained separately in the [Packmetry development repository](https://github.com/packmetry/packmetry). No development source, raw planning conversations or datasets are copied into this showcase. Public visibility does not by itself grant a software or documentation license.
