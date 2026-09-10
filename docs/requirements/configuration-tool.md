# Configuration Tool requirements

Agreed future scope for the separate Configuration Tool.

### REQ-008: Separate Configuration Tool (future scope)

A separate Configuration Tool must be created later to help users prepare and preview the parameters for an MCP server entry in the ChatGPT Codex configuration file.

The Configuration Tool may obtain or discover the Unreal installation path from the environment and include it as an explicit `--editor` parameter in the preview when lifecycle control is enabled. This discovery belongs to the Configuration Tool; the MCP server requires an explicit project path and, when lifecycle control is enabled, an explicit Unreal installation path. The preview must represent the independent `--writable` and `--editor` options.
