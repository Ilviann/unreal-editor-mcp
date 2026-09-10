# Entity inspection and authoring tool catalog review

This document tracks the agreed read-only entity and initial authoring tool catalogs. Detailed specifications and supported authoring families remain to be reviewed individually.

## Agreed tool specifications

- [Asset search](asset-search.md): `asset_search` searches by name mask, base class, and saved references in both directions. Its request structure, matching rules, pagination, YAML results, metadata, and traversal-limit policy are agreed. Numeric traversal thresholds await implementation profiling.

## Agreed read-only entity tool list

| Tool | Agreed purpose |
| --- | --- |
| `asset_search` | Agreed: find assets by name mask, base class, and saved references, with scopes, pagination, and brief/metadata results. |
| `asset_inspect` | Inspect any asset type, including data, Gameplay Ability System, UI, and external-plugin assets. See [asset inspection](asset-inspection.md). |
| `level_open` | Open a specified level for inspection; stop and warn if the current level has unsaved changes. See [level tools](level-inspection.md). |
| `level_inspect` | Inspect the currently opened level's settings, objects, World Partition information, and other agreed level details. See [level tools](level-inspection.md). |

All four tools are available without `--writable`. This is the entity tool list; previously agreed project/editor status and optional lifecycle control remain separate server capabilities.

`game_data_inspect` is folded into `asset_inspect`. Separate `asset_references` and `blueprint_action_catalog` tools are not retained in the agreed read-only entity tool list. Reference search is covered by `asset_search`; any additional reference evidence or Blueprint action discovery needed for authoring must be addressed when defining the retained tools.

### Entity coverage to review

Inspection of any asset type is agreed; the depth and specialized semantics for each family remain to be specified. Authoring support requires separate decisions and does not follow automatically from inspection support. These groupings do not prescribe native plugin or module boundaries.

- Gameplay Blueprints, framework classes, Actor Component Blueprints, and Blueprint Interfaces.
- Blueprint graphs, nodes, pins, members, functions, macros, events, components, and class defaults.
- UMG Widget Blueprints, widget trees, layout, styles, and bindings.
- Animation Blueprints, animation layers, state machines, and transitions.
- Data Assets, Primary Data Assets, user-defined structs, and Data Tables.
- Maps, placed actors, actor components, World Partition, and Data Layers.
- Gameplay Ability System assets and supporting Blueprint families.
- CommonUI widgets and specialized UI semantics.
- Enhanced Input actions, mapping contexts, triggers, and modifiers.
- Unreal AI Behavior Trees, Blackboards, Environment Queries, and custom node Blueprints.

These candidates do not automatically include runtime/PIE inspection, media payloads, or arbitrary object traversal. Such scope needs separate review.

## Agreed initial authoring tool list

The [authoring catalog requirements](authoring-tools.md) define ten tools requiring `--writable`: `asset_create`, `asset_update`, `asset_graph_update`, `asset_compile`, `asset_save`, `asset_delete`, `level_create`, `level_update`, `level_save`, and `project_settings_update`.

## Remaining scope decisions

- The four read-only entity tools and ten initial authoring tools are agreed. Review their individual capabilities and schemas using these shared tool boundaries.
- Decide which entity families need authoring support. The current candidate list does not establish dedicated authoring coverage for GAS, CommonUI, Enhanced Input, AI, Animation Blueprints, Data Assets, Materials, Niagara, PCG, splines, MVVM, or Widget Animation timelines. These are additional scope candidates, not exclusions already agreed for the new project.

### Supporting and runtime capabilities

Editor status, startup, shutdown, and restart already have agreed requirements in the [editor lifecycle specification](editor-lifecycle.md). The shutdown tool is named `editor_close`: without `--writable` it stops and warns on unsaved assets or levels; with `--writable` it saves all changes before closing. It requires `--editor` in either mode. The remaining lifecycle tool names and grouping are separate from the entity tool list.

Other candidates to account for when reviewing the overall server catalog are `capabilities`, `operation_status`, and `operation_cancel`. General operation tracking and cancellation are not approved merely because lifecycle MCP Tasks support is agreed.

Project-file generation/build tools, PIE control, runtime inspection/testing, and screenshots are additional unreviewed areas. They are not silently included in the entity tool list.

## Specification topics for each accepted capability

- Purpose and supported entity families, including local/inherited and asset/instance distinctions.
- Target discovery, stable identity, path/mount scope, and selection of nested entities.
- Inputs, defaults, validation, and response structure with examples.
- Completeness, pagination, graph boundaries, and resource limits.
- Required editor state and interaction with user edits or other in-progress operations.
- For authoring: preconditions, transactions, compile/save behavior, persistence, and failure recovery.
- Errors, asynchronous behavior where needed, and acceptance criteria.

Resume inspection specifications and then the corresponding authoring specifications. Decide whether an inspection response must supply all identities and preconditions needed by its related authoring tools.
