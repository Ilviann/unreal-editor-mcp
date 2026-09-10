# Project requirements

This folder records agreed requirements for the new Unreal Editor MCP implementation and candidates awaiting review. Capabilities are reviewed individually; unreviewed capabilities and previous implementation choices are not requirements. Requirement IDs remain stable when documentation is reorganized.

## Contents

- [MCP server](server.md): Agreed local usage, stdio transport, fixed project configuration, and independent write/lifecycle access options.
- [Editor lifecycle](editor-lifecycle.md): Agreed editor status, startup, mode-dependent `editor_close`, restart, and MCP Tasks/fallback behavior.
- [Configuration Tool](configuration-tool.md): Agreed future configuration preview tool and its environment discovery scope.
- [Inspection output](inspection-output.md): Shared YAML responses and error messages carried in standard MCP text content.
- [Asset search](asset-search.md): Agreed tool request, matching rules, content scopes, stateless pagination, YAML results, metadata, and traversal-limit policy; numeric thresholds await implementation profiling.
- [Asset inspection](asset-inspection.md): Agreed inspection of any asset type, with inspection depth and detailed contracts awaiting review.
- [Level navigation and inspection](level-inspection.md): Agreed read-only level opening and current-level inspection, including refusal to leave unsaved level changes.
- [Authoring tool catalog](authoring-tools.md): Agreed ten-tool authoring catalog using `update` naming, with detailed operations and supported families awaiting review.
- [Entity tool catalog review](entity-tools.md): Agreed four-tool read-only entity catalog and ten-tool initial authoring catalog, plus remaining capability and specification topics.

## Open decisions

- MCP protocol/Tasks extension compatibility and verification with intended clients, including Codex.
- Whether multiple Unreal Editor instances may open the same project during MCP use.
- Build coordination when projects share an engine installation or plugin files.

The planned asset-search requirements review is complete; numeric traversal thresholds remain an implementation follow-up. The four read-only entity tools and ten initial authoring tools are agreed. Remaining capability and specification decisions are tracked in the entity tool review document.
