# Project Status and Evidence Scope

[Back to overview](../README.md)

## Audited source state

This case study is grounded in [packmetry/packmetry at commit fced7e5](https://github.com/packmetry/packmetry/tree/fced7e578886587c3bef47e14cbb18489a578ca7), the inspected main-branch state on 7 October 2026. GitHub reported the development repository as public. The npm package's private flag does not make the GitHub repository private; its declared license is UNLICENSED.

Current source, tests and configuration take precedence over accepted ADRs and implementation logs for implementation claims. The project bootstrap establishes brand and intended boundaries, not completion of its roadmap. CartonLab references are legacy naming unless explicitly historical.

## Evidence navigation

- [Core contracts, solver and verification](https://github.com/packmetry/packmetry/tree/fced7e578886587c3bef47e14cbb18489a578ca7/src/core)
- [Browser adapters and workspaces](https://github.com/packmetry/packmetry/tree/fced7e578886587c3bef47e14cbb18489a578ca7/src)
- [Current automated tests](https://github.com/packmetry/packmetry/tree/fced7e578886587c3bef47e14cbb18489a578ca7/src/test)
- [Accepted decision records](https://github.com/packmetry/packmetry/tree/fced7e578886587c3bef47e14cbb18489a578ca7/docs/decisions)
- [Package/configuration evidence](https://github.com/packmetry/packmetry/blob/fced7e578886587c3bef47e14cbb18489a578ca7/package.json) · [CI workflow](https://github.com/packmetry/packmetry/blob/fced7e578886587c3bef47e14cbb18489a578ca7/.github/workflows/ci.yml)

## Capability matrix

| Capability | Current evidence classification |
| --- | --- |
| Static Astro site, React Personal/Business workspaces and SEO foundations | IMPLEMENTED; typecheck/static build passed |
| Canonical millimeter/gram inputs and runtime domain validation | IMPLEMENTED and TESTED |
| Greedy baseline with bounded carton-order candidates and exact deduplication | IMPLEMENTED and TESTED; heuristic, not globally optimal |
| Independent candidate checking before ranking/canonical construction | IMPLEMENTED and TESTED within the normal orchestration path |
| Boundary, overlap, references, quantity, rotation and inventory checks | IMPLEMENTED and TESTED |
| Gross-weight limits | IMPLEMENTED and TESTED when all required physical weights are known; otherwise unknown, not rejected |
| Need Boxes, Have Boxes, Hybrid | IMPLEMENTED and TESTED; all exposed in Personal, only Have Boxes exposed in Business |
| Business SKU and advanced carton fields over shared engine | IMPLEMENTED and TESTED |
| Balanced, Fewest Boxes, Least Empty Space, Lowest DIM Weight | IMPLEMENTED and TESTED; Business UI choices with required-data gates |
| Easier to Carry and Lowest Carton Cost | IMPLEMENTED and TESTED in core; not exposed as Business choices |
| Existing-inventory-first ranking | MODELED BUT NOT IMPLEMENTED; Hybrid staging is different |
| Canonical selected/alternative plans, metrics and derived status | IMPLEMENTED and TESTED; ordinary mutable objects |
| Canonical explanation records | TYPE EXISTS; currently empty; UI rationale derives from metrics |
| DIM/actual/chargeable mass with explicit units | IMPLEMENTED and TESTED; no live carrier-rate engine |
| Three.js canonical placement rendering and text/legend companions | IMPLEMENTED; pure-model/SSR tests, no real-browser/GPU validation |
| Local histories, carton library, preferences and Business JSON backup | IMPLEMENTED and TESTED with browser-storage stand-ins |
| Fragile, padding, spacing and stackable constraints | MODELED BUT NOT ENFORCED |
| Worker execution, cancellation and search-budget behavior | DEFERRED; async adapter alone does not implement them |
| Standalone saved product catalog, CSV and batch processing | PLANNED/FUTURE; recent input/project storage is not these features |
| Cloud accounts/sync, analytics, carton portfolio intelligence and external integrations/API | PLANNED/FUTURE; absent from current core application |
| Carrier presets with verified provenance; shipping rates/labels | PLANNED/FUTURE or outside current scope; no current service claim |
| Exhaustive optimum, physical stability guarantees, full browser/accessibility certification | NOT ESTABLISHED |

## Validation and maturity

The fresh audit run passed current tests, Astro typecheck and static build. This supports a tested browser-first engineering baseline, not a production-service or physical-packing certification. The build emits a large-client-chunk warning. Interactive browser coverage, measured performance and real packing trials remain open.

A successful CI validation run for the inspected commit was observed. No source deployment workflow or authoritative live-deployment record was established; configuration supports static output and the Packmetry.com identity, not a verified hosting-provider claim.

## Meaningful divergence from older material

- ADR-005's single-candidate baseline was revised by accepted ADR-011 and current implementation.
- ADR-008's historical unsupported-DIM statements are superseded by ADR-012 and current code/tests. Existing-inventory-first ranking is still unsupported.
- Some alternative UI wording still describes DIM semantics as future despite implemented DIM ranking.
- The phased implementation log names SEO as a next phase, while source already contains metadata, robots, sitemap and guide foundations. These foundations do not prove the full SEO phase complete.

No source files were changed to reconcile these discrepancies. The showcase deliberately reports current behavior and preserves the distinction between model, intended architecture and tested implementation.

## Showcase status

This is documentation prepared for publication review; it is not a source mirror. Primary evidence now replaces the earlier missing-source block. Local diagram, link and safety results are recorded separately in the local audit record. No source or showcase was published or pushed during this audit. Licensing remains an owner decision; public visibility alone is not permission to reuse private or unlicensed material.
