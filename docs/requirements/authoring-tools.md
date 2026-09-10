# Authoring tool catalog requirements

This document records the agreed initial authoring tool catalog. Detailed operations, schemas, and supported asset families remain to be specified.

### REQ-030: Shared authoring tool catalog

The MCP server must provide the following authoring tools when `--writable` is enabled. Tools that change existing state use `update` in their public names rather than `edit`.

| Tool | Responsibility |
| --- | --- |
| `asset_create` | Create an asset of a supported type at a specified destination, including supported Blueprint and data asset types. |
| `asset_update` | Update supported non-graph asset structure and values, such as defaults, declarations, components, widget trees, struct schemas, and Data Table rows. |
| `asset_graph_update` | Update supported asset graphs: nodes, pins, connections, and complete logic blocks. |
| `asset_compile` | Compile an asset type that supports compilation and return diagnostics. |
| `asset_save` | Explicitly save asset changes. |
| `asset_delete` | Delete an eligible asset or inactive map. |
| `level_create` | Create a new level. |
| `level_update` | Update current-level settings, placed actors, and actor components. |
| `level_save` | Explicitly save current-level changes. |
| `project_settings_update` | Update approved project settings, initially considering GameMode and GameInstance defaults. |

Generic tool names do not imply universal authoring support. Supported asset classes and operations must be agreed individually. The agreed ability to inspect any asset does not automatically grant the ability to create or modify every asset type.

Opening an existing level remains the responsibility of the agreed read-only `level_open` tool. Editor startup, close, and restart remain separately controlled by `--editor`.

## Open specification decisions

- Supported asset families and creation/update coverage, including external-plugin assets.
- Operation discovery for each target, especially graph-node creation after removing the separate Blueprint action-catalog tool.
- Graph update granularity, complete-block replacement, and layout responsibilities.
- Compile/save behavior, transactions, failure recovery, and reference handling.
- Scope of level updates and project settings.

These are specification topics for later review; none adds an implementation requirement by itself.
