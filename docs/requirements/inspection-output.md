# Inspection tool output requirements

This document defines the shared output format for `asset_search` and other inspection tools. Individual tool specifications define the contents of their YAML responses.

## Agreed requirements

### REQ-024: YAML inspection results in MCP text content

Inspection tools must return their output in one MCP text-content block with `type: "text"` and a `text` string:

- On success, `text` contains the tool's YAML response directly, without Markdown code fences. MCP `isError` is false or omitted.
- On tool execution failure, `text` contains an error message and MCP `isError` is true. The error message need not be YAML.

Do not add custom `result` or `failed` payload fields or duplicate the response in `structuredContent`. The standard MCP tool-result and JSON-RPC envelopes still apply; this requirement describes their tool content rather than replacing protocol framing. Protocol-level errors retain the standard JSON-RPC error format.

The text-content block has this shape (illustrative YAML only; individual result schemas are specified separately):

```json
{
  "type": "text",
  "text": "items:\n  - /Game/Actors/BP_Door.BP_Door\n"
}
```

Pagination, completeness, and warning information required by an inspection tool must be expressed within its YAML response. The [asset-search requirements](asset-search.md) define that tool's agreed YAML envelope.
