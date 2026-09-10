# Asset inspection requirements

This document records the agreed scope of `asset_inspect`. Detailed inputs, outputs, and inspection depth remain under review.

Responses follow the shared [inspection output requirements](inspection-output.md).

## Agreed requirements

### REQ-027: Inspect any asset type

The read-only entity tool `asset_inspect` must support inspection of any asset type available in the configured project's Unreal Editor environment, including data assets, user-defined structs, Data Tables, Gameplay Ability System assets, UI assets, and assets supplied by external plugins.

Struct and Data Table inspection belongs to `asset_inspect`; there must not be a separate public `game_data_inspect` tool. The tool must be available without `--writable`.

## Open specification decisions

- The guaranteed inspection depth for arbitrary asset types, including unfamiliar external-plugin assets, and the role of generic versus specialized inspection.
- Target identifiers, selectors, result detail, limits, and other tool-specific contracts.
