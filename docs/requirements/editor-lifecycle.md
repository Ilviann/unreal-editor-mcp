# Unreal Editor lifecycle requirements

Agreed requirements for project-specific editor status, startup, shutdown, restart, and asynchronous operation responses.

### REQ-004: Project-specific editor availability

The MCP server must remain responsive when Unreal Editor is closed or unresponsive. It must report whether Unreal Editor is running with the specified Unreal project; an editor running another project does not satisfy this requirement.

Availability reporting must distinguish an editor process running the specified project from that editor being ready to handle requests. When the state cannot be determined, report that uncertainty rather than claiming the editor is stopped or ready. Editor-dependent requests must return an explicit availability error when they cannot proceed.

### REQ-005: Launch Unreal Editor with the specified project

When editor lifecycle control is enabled by `--editor`, the MCP server must expose one startup tool to start Unreal Editor from its explicitly configured Unreal installation with its fixed target project, including when no editor is running. Startup must run in the background while the MCP server remains responsive.

The startup tool must adapt its response to client capabilities:

- When the client advertises the supported MCP Tasks extension, return a task handle promptly. The client can follow the task through the standard task protocol until it yields a final `ready` result or an error.
- Otherwise, return an ordinary tool result promptly with `starting` after launch has been initiated. The client can check readiness through the status tool.

An ordinary status tool must be available to all clients regardless of MCP Tasks support and return promptly with the current project/editor availability.

`ready` means the configured project is open in Unreal Editor and its editor integration can accept requests. Creating an editor process alone does not establish readiness. A task handle must never be returned to a client that has not advertised the required task capability.

Repeated startup calls must reuse the existing editor or in-progress startup for the configured project, rather than launching another editor instance. If that editor is ready, return `ready` immediately. If startup is still in progress, follow that startup through MCP Tasks when supported, or return `starting` for ordinary status polling. An existing editor that is unresponsive or whose readiness is uncertain must not be treated as absent to justify another launch.

This behavior checks the target Unreal Editor state and does not require checking for other MCP server instances.

### REQ-009: Mode-dependent editor close

When editor lifecycle control is enabled by `--editor`, the MCP server must expose `editor_close` to gracefully close the current Unreal Editor instance running its configured project. It must not target unrelated editor instances.

The tool's behavior depends on the server's fixed write-access mode:

- Without `--writable`, stop and warn if any assets or levels have unsaved changes. Do not save, discard changes, or close the editor in this case.
- With `--writable`, save all unsaved asset and level changes in the target editor, then close it. Saving is part of the close operation. If any required save fails or unsaved changes remain, stop and report the problem rather than close and lose changes.

Writable-mode use assumes that all changes in the target editor were made by the connected LLM agent. The close operation saves all changes rather than attempting to select changes by author.

Active work that prevents a safe save or exit must cause the operation to stop with a clear reason. The tool must not discard changes or force-terminate the editor. Successful closure requires confirmation that the target editor process has exited.

### REQ-010: Restart Unreal Editor

When editor lifecycle control is enabled by `--editor`, the MCP server must expose the ability to restart the Unreal Editor running its configured project. Restart must compose graceful shutdown, verification that the old editor process has exited, and launch of the same project using the configured Unreal installation.

Restart must use the same mode-dependent close policy as `editor_close`: without `--writable`, stop and warn on unsaved assets or levels; with `--writable`, save all changes before closing. If saving fails, unsaved changes remain, active work prevents a safe exit, or process exit cannot be confirmed, restart must stop and report the problem without launching a replacement editor. Restart succeeds only when the new editor has opened the configured project and its integration is ready to accept requests.

The MCP server must remain responsive throughout restart.

### REQ-011: Consistent lifecycle operation responses

Startup, shutdown, and restart must all use the same capability-based response behavior. For an accepted operation that remains in progress:

- If the client advertises the supported MCP Tasks extension, return a task handle promptly and expose progress and the final result through the standard task protocol.
- Otherwise, return an ordinary tool result promptly with the operation's in-progress state. The client can follow progress and obtain the outcome through ordinary status tool calls.

Status reporting must distinguish startup, shutdown, and restart progress and make failures explicit. Startup and restart succeed only on verified readiness; shutdown succeeds only once the target editor process has exited. Immediate completion or refusal may return its result directly.

The MCP server must remain responsive during all lifecycle operations. The ordinary status tool must be available regardless of MCP Tasks support. Shutdown in this response policy refers to `editor_close`; in writable mode, its asynchronous work includes saving all changes before closing.
