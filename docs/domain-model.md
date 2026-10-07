# Domain Model and Runtime Semantics

[Back to overview](../README.md)

## Normalization boundary

The core uses millimeters for length and grams for mass. Conversion functions support mm, cm, m, inches and feet; mass supports grams, kilograms, ounces and pounds. Dimensions preserve their length/width/height axes. Values use JavaScript numbers, with finite positive validation and geometric tolerance, rather than exact decimal arithmetic.

Personal stores canonical values while changing display units, avoiding repeated conversion of rounded display text. Business currently uses millimeter/gram fields. Strict TypeScript contracts describe shape; domain constructors and validators provide runtime checks. A TypeScript interface alone does not validate arbitrary caller input.

## Implemented concepts

| Concept | Runtime behavior | Important boundary |
| --- | --- | --- |
| Item | Validated identifier, positive dimensions and positive integer quantity; optional name, SKU and unit weight | Unknown unit weight remains unknown |
| Carton | Internal dimensions plus optional external dimensions, tare, gross limit, cost, availability and stock fields | Internal geometry and external DIM measurements serve different purposes |
| Inventory | Available/stock bounds restrict opened carton instances; the tighter supplied bound applies | An absent bound means unrestricted availability; zero means unavailable |
| Rotation | Axis-aligned permutations filtered by policy | No arbitrary-angle or irregular-object packing |
| Placement | Item identifier and instance index, lower-corner coordinates, oriented dimensions and rotation | Verification binds each placement to input geometry and quantity |
| Packed carton | Carton reference plus a set of placements and derived metrics | Used-carton instance counts are checked against supplied bounds |
| PackingPlan | Selected checked placements, unplaced instances, objective, metrics, derived status and metadata | Canonical does not mean complete, immutable or globally optimal |

The domain validates positive supplied weights and limits, and nonnegative supplied carton costs. It does not assign a currency or a shipping price. Identifier validation requires nonempty identifiers; callers must supply coherent identities for their input collections.

## Rotation and handling

The core supports unrestricted axis-aligned rotations, fixed orientation, upright orientation and vertical-axis-only rotation. The latter policies preserve height while allowing the horizontal axes to swap. Business exposes unrestricted, upright and fixed options; not every core policy is a separate UI choice.

Fragile, padding, spacing and stackable fields have normalized model values but no corresponding geometric/physical enforcement in the current baseline or verifier. These are modeled constraints, not active protection. There is no verified model of support, crush resistance, center of gravity or load-bearing stacking.

## Quantity and status

Each repeated item unit is identified by its input item identifier and instance index. The verifier requires every requested instance to appear exactly once across placements and unplaced records; duplicates, omissions and out-of-range indices fail verification.

Canonical construction derives status from coverage and unplaced reasons rather than trusting a solver's status label. The result contract can represent feasible, partial, infeasible and limit-related outcomes. The built-in baseline does not implement an explicit search-budget/cancellation mechanism merely because a limit status is modeled.

## Serialization contracts

PackingPlan JSON export serializes the canonical result. Its runtime validator checks result structure and metric ranges, but does not rerun input-bound geometric verification. Exported JSON is not a certificate of feasibility for arbitrary supplied data.

Business project JSON is a separate versioned, validated input-backup format. Import restores the workspace for recalculation; it does not import a new trusted PackingPlan. See [persistence](persistence-and-privacy.md).
