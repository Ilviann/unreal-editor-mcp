# MCP server requirements

Agreed requirements for local usage, transport, startup configuration, and access options.

### REQ-001: Local client

The MCP client must run on the same machine as the Unreal Editor it uses. Remote MCP clients are outside the supported scope.

### REQ-002: Single client per project

The supported usage model permits only one MCP client at a time for each Unreal project. Exclusivity applies to the project, not the whole machine. Separate projects may each have their own MCP client and Unreal Editor instance on the same machine.

This is a supported usage constraint, not an enforced exclusivity guarantee. Do not check whether another MCP server is already running for the same project or implement admission checks or locks to enforce this constraint. Duplicate-server detection and second-client handling are deferred.

Project-level client exclusivity does not establish that simultaneous builds or writes to shared engine/plugin files are safe; build coordination remains a separate open decision.

### REQ-003: MCP over stdio

The MCP client must communicate with the MCP server over standard input and standard output (stdio). This requirement does not select the implementation language, libraries, or the communication mechanism between the MCP server and Unreal Editor.

### REQ-006: Required fixed project at startup

MCP server startup must require a parameter containing the path to the target `.uproject` file. A containing directory alone does not satisfy this parameter, and the server must not infer a project when the parameter is omitted.

The target project is fixed for the lifetime of the MCP server. Availability reporting, editor launch, and all project operations must use that project. Tool calls must not switch or override the target project; using a different project requires starting a server with that project's `.uproject` path.

### REQ-007: Optional editor lifecycle control through an explicit installation

The optional startup parameter `--editor <path-to-editor-folder>` enables Unreal Editor lifecycle control. Its value must identify a specific Unreal Editor installation folder, rather than an editor executable file. Without this parameter, startup, shutdown, and restart tools must not be available; project/editor status inspection remains available.

When supplied, the server must use that installation to launch Unreal Editor. It must not automatically discover or select an installation from environment variables, project associations, or other sources. Tool calls must not override the configured installation. The editor installation path is required only when enabling lifecycle control; the `.uproject` path remains required for every server startup.

### REQ-012: Independent write access and editor control options

The startup flag `--writable` enables tools that modify project content. Without `--writable`, inspection tools and the explicitly approved `level_open` navigation tool are available. Editor lifecycle tools are independently enabled by `--editor`.

`--editor` does not grant project write access, and `--writable` does not enable editor lifecycle control. These permissions are fixed at startup and must not be enabled through tool calls. Tools disabled by these options must not be published or callable.

| Startup options | Available tool categories |
| --- | --- |
| Neither option | Inspection, including project/editor status, and `level_open`. |
| `--writable` only | Inspection, `level_open`, and project-content modification. |
| `--editor <path-to-editor-folder>` only | Inspection, `level_open`, and editor startup, shutdown, and restart. |
| Both options | Inspection, `level_open`, project-content modification, and editor startup, shutdown, and restart. |
