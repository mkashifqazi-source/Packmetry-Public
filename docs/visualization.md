# Visualization and Result Consumers

[Back to overview](../README.md)

## Placement truth remains upstream

~~~mermaid
flowchart LR
    P[Canonical PackingPlan] --> S[Text summary and metrics]
    P --> I[Inventory impact]
    P --> A[Alternative-plan display]
    P --> M[Pure visualization model]
    M --> T[Three.js scene and orbit controls]
    P --> E[Plan JSON export and local plan history]
    B[Business input backup] --> R[Restore inputs and recalculate]
~~~

Each alternative is itself a canonical plan. Selecting an alternative changes the displayed plan. Business input backups are deliberately outside the result-authority path: restoring inputs requires recalculation.

## Scene adaptation

The viewer receives a PackingPlan and optional item labels. A pure visualization-model function selects a packed carton and converts placement lower-corner coordinates into centered Three.js coordinates. The axis mapping accounts for the scene's vertical axis; oriented placement dimensions already encode permitted rotation.

The renderer draws the carton's internal bounds, colored item solids and outlines. It does not call a solver, rearrange items, invent padding or choose new rotations. Display colors, labels, camera position and carton selection are presentation state, not new packing truth.

## Interaction and lifecycle

Per-carton controls choose which packed carton is shown. Orbit controls support rotating and zooming the scene. A legend names represented items and counts; canonical text summaries accompany the 3D view. There is no implemented drag-to-repack or item-editing authority in the renderer.

The component responds to window resize, adapts canvas width and caps rendering pixel ratio. Cleanup cancels the animation loop, removes listeners and disposes controls, geometries and materials. WebGL initialization failure has an error display; the text result remains available.

These are implemented mechanisms, not proof of performance across mobile GPUs or full accessibility compliance. Canvas interaction has not been independently audited as a complete keyboard/screen-reader geometry experience. Text and legends provide partial alternatives rather than reproducing every spatial relationship.

## Verification scope

Tests cover pure coordinate mapping, carton selection/model output, legend/text rendering and source-level integration with canonical results. The audit did not run a real interactive browser/GPU E2E suite. The static build's large-client-chunk warning also leaves bundle/performance work open.
