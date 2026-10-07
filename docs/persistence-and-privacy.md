# Persistence and Data Boundaries

[Back to overview](../README.md)

## Implemented local stores

| Mechanism | Stored material | Scope |
| --- | --- | --- |
| IndexedDB | Personal recent item inputs and canonical recent plans | Local bounded history; not cloud synchronization |
| IndexedDB | Business carton library | Saved definitions; not an inventory transaction ledger |
| IndexedDB | Business recent projects | Product/carton inputs, objective and optional DIM settings; not a trusted saved PackingPlan |
| localStorage | Personal display-unit preference; Business objective and handling preferences | Small local preferences |

The current IndexedDB schema uses version 1 with initial store creation. There is no demonstrated multi-version data-migration system. Failure-tolerant adapters report unavailable storage; packing itself does not require successful persistence. Storage can be cleared, evicted or unavailable in a browser context and is not application-encrypted.

Personal recent inputs are not a standalone managed product catalog. Business project snapshots contain product rows, but do not implement a separate saved-product-catalog service. Inventory-impact results do not automatically decrement saved carton stock.

## Import, export and sharing

Business JSON backups use an explicit versioned format and runtime validation of nested input records, objective and DIM settings. Unsupported versions and malformed records are rejected. Import restores input state and clears stale results; it does not make imported placements authoritative or silently save an imported project as validated output. Persistence initialization guards project-save identity/readiness behavior.

Personal plan downloads serialize canonical PackingPlan results without the Business backup wrapper. The two JSON formats have different purposes. The PackingPlan structural validator is not a substitute for geometric verification against original inputs.

Sharing uses browser-supported file/text sharing with a clipboard fallback. It does not create a hosted public share link. File downloads, clipboard use and user-selected sharing can move data outside the browser origin by explicit user action.

## Browser-first boundary

The inspected core packing application has no mandatory backend, account requirement, server-side solver, cloud database, paid API or application telemetry integration. Inputs, computation and these local stores remain in the browser unless the user exports/shares them.

This is a concrete data-flow statement, not a complete privacy guarantee. Hosting still serves pages/assets and may observe request metadata; local device users, browser extensions and origin permissions remain relevant. Data are not automatically backed up or synchronized across devices.

Cloud accounts, remote persistence, CSV/batch integration and hosted sharing are future scope, not inferred from the local adapters. No live deployment-provider claim is made from static build configuration alone.
