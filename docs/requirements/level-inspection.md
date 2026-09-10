# Level navigation and inspection requirements

This document records the agreed read-only level tools. Detailed request and response specifications remain under review.

Inspection responses follow the shared [inspection output requirements](inspection-output.md).

## Agreed requirements

### REQ-028: Open a level for inspection

The public `level_open` tool must open a specified level in Unreal Editor for inspection. It must be available without `--writable`.

If the currently opened level has unsaved changes, the operation must stop and warn the caller. It must not switch levels, implicitly save, or discard those changes.

This tool is an explicitly permitted navigation action in read-only mode. It does not grant project-content write access or permission to launch or shut down Unreal Editor.

### REQ-029: Inspect the currently opened level

The public `level_inspect` tool must inspect the currently opened level, including its settings, objects, and World Partition information. It must be available without `--writable`.

Level inspection targets the current level; opening another level is the responsibility of `level_open`. Asset discovery belongs to `asset_search`.

## Open specification decisions

- Level identifiers, navigation results, and editor-state checks beyond the agreed unsaved-current-level refusal.
- Detailed level/entity coverage, selectors, pagination, and limits.
