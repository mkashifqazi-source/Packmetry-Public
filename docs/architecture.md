# Architecture

[Back to overview](../README.md)

## Application boundary

Astro produces a static website with landing, Personal, Business, guide, contact and privacy pages. React islands host interactive packing workspaces. Site metadata, canonical URLs, robots and sitemap output are implemented; these foundations do not establish search performance or completion of every SEO roadmap item.

No server adapter, mandatory account service, cloud database or server-side packing endpoint is present in the inspected application. Loading the hosted site still involves network requests for pages and assets. A static build is evidence of deployable artifacts, not evidence of a particular live deployment.

## Engine boundary

The core domain, units, solver, verification and workflow modules do not depend on UI frameworks, Three.js or browser storage. UI inputs enter normalized constructors before planning. The solver adapter returns candidate claims, independent verification reports on those claims, selection compares eligible results, and construction creates canonical PackingPlan values.

The adapter has an asynchronous interface, but the built-in heuristic computes on the calling thread. Worker execution, cancellation and an explicit runtime search budget are not implemented just because the interface could support future adapters.

## Two experiences over one engine

Personal offers metric/imperial input display and all three box workflows. Business adds SKU-oriented item inputs, advanced carton fields, objective selection, local carton libraries and project backups. Business currently presents Have Boxes only. Both use the same canonical units, baseline solver, verifier and plan construction path.

Business uses collapsible native disclosure sections for optional DIM settings and project/backup controls. This progressive disclosure changes presentation, not engine authority.

Differences are in input presentation and workflow orchestration, not separate packing engines. Hybrid calls the shared planner several times for distinct subproblems; it does not grant the UI authority to splice unchecked placements together.

## Result and adapter boundaries

Canonical plans carry placements, unplaced instances, recomputed status, metrics, objective and solver metadata. Summaries, alternatives, inventory-impact displays, plan exports and the 3D view consume those results. Business project backups instead preserve input state for later recalculation.

Browser adapters isolate IndexedDB, localStorage, file downloads and sharing from the engine. The renderer adapts canonical coordinates to scene coordinates without searching for a new arrangement. See [pipeline](packing-pipeline.md), [visualization](visualization.md) and [persistence](persistence-and-privacy.md) for those distinct contracts.

## Architectural limits

Verifier independence means separate checking logic in the same application with shared domain types and rotation utilities. It is not a process boundary or protection against malicious callers forging a report. Canonical plans are ordinary objects, not immutable signed authorizations. The normal orchestration path supplies the checked inputs/report and rejects invalid candidates; external callers must preserve that contract.

No current architecture evidence supports cloud synchronization, fulfillment integration, inventory transactions or a live rate engine.
