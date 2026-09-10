# Previous implementation: discussion reference

These historical notes preserve useful lessons and unreviewed topics for the next discussion. They are not requirements for the new implementation. The agreed [requirements](requirements/index.md) take precedence. Authoring tools will be rebuilt from the ground up, with a simpler design.

## Historical context

The inspected snapshot was dated 2026-09-10, with server/base-plugin version 0.53.0 and source revision `c94bd214fcde3e7cd0cde3ec64797f1536bb1d84`. Its behavior was reported from source, schemas, tests, and documentation; native acceptance was not rerun for that inventory.

The previous implementation targeted Unreal Engine 5.8+ and Python 3.10+. Windows was the mandatory native release platform, macOS had implemented support with outstanding verification, and Linux was outside the supported scope. Platform and version requirements for the new project still need their own decision.

## Authoring and inspection lessons to discuss

- **Inspection must enable the intended updates.** The previous semantic inspector did not expose every reconstruction precondition required by complete graph replacement. Native GUIDs alone did not provide ownership sets, crossing links, or complete replacement boundaries. Decide how clients obtain the information needed by each retained update operation.
- **Identity changes matter.** Compilation, node reconstruction, Undo/Redo, and reload can change usable identities. Local versus inherited members, components, overrides, and interface implementations need explicit semantics.
- **Graph inspection is not ordinary collection paging.** Partial graphs need meaningful dependency boundaries and explicit omissions. Complete function, macro, and event-handler replacement needs to preserve unrelated and shared nodes. The previous system used scratch compilation and automatic layout; these are approaches to evaluate, not inherited requirements.
- **Saving and rollback need precise meanings.** Previously, many graph/widget/actor updates remained dirty, while creation and data changes could compile and save. Transactions did not provide filesystem atomicity, multi-package saves could partially succeed, and deletion was not undoable. Define each new operation's actual outcome and persistence behavior.
- **Lost responses do not establish failure.** A mutation may have completed when its reply was lost. Discuss retry behavior, reconciliation, and safe cancellation without automatically retaining the previous operation-ledger architecture. MCP Tasks and mutation deduplication solve different problems.
- **Reference search and deletion evidence differ.** Saved dependency queries can be package-granular and do not account for all live references or runtime-built paths. The old deletion path also checked live/Undo references and package ownership. The agreed saved-only search is not, by itself, a complete deletion contract.
- **World Partition affects inspection and persistence.** Unloaded actor descriptors can support discovery without loading whole regions. Exact inspection may require a scoped load. Map changes can involve external actor/object packages and build data; deleting an inactive map may require handling its owned package set.
- **Asset-family semantics need deliberate coverage.** Topics included typed defaults and containers, registered Gameplay Tags, replication/RepNotify/RPC relationships, widget slots and bindings, inherited widget trees, struct dependencies, and Data Table schema/row consistency. These should inform operation discussions without copying the old allowlists or numeric limits.

## Architecture and extension topics

The previous design used a standard-library Python stdio server and an editor-only C++ plugin with authenticated loopback HTTP. Native work was dispatched to the Unreal Game thread. It had typed command/family registries, a shared authoring layer, and separate native modules. These are historical choices; the new bridge, languages, libraries, and module boundaries remain to be discussed.

Optional GAS, CommonUI, Enhanced Input, and AI companion plugins contributed specialized inspection only. Animation Blueprint inspection was also available. Their existence did not imply authoring support. For the new any-asset inspector, distinguish generic metadata/properties from specialized semantics and decide how external-plugin types are handled.

Other useful design topics are protocol/version compatibility, truthful capabilities when dependencies are missing, strict argument validation, protocol-only stdout, Unicode handling, bounded work, and safe YAML serialization that preserves scalar types. Select the simplest mechanisms that satisfy agreed requirements.

## Support tooling and verification topics

- Packaging used Unreal AutomationTool `BuildPlugin`, descriptor/binary validation, and separate platform builds. Discuss project-local versus engine-level installation, debug symbols, and rollback of failed deployments.
- A Windows deployment helper could discover engines, enable selected plugins, and preview host configuration. For the new project, the agreed separate Configuration Tool may discover paths; the MCP server must continue to use explicit configuration.
- Tests covered offline protocol/schema behavior and native Unreal Automation, plus real-transport headless scenarios. Useful scenarios include inspection without content changes, update read-back, save/restart persistence, failed saves, lost replies, stale identities, and preservation of unrelated content.
- Normal/adaptive, forced-unity, and non-unity builds helped reveal include/dependency problems. Cross-platform process launch and packaging need separate verification from language-level unit tests.
- Use disposable projects and representative asset families to evaluate behavior and choose practical resource limits. Old limits and test harness structure are not requirements for the new project.

## Historical unresolved reports

These are regression scenarios to investigate, not established defects in the new project:

- Intermittent crashes during requested editor restart were reported; platform, version, and cause were not established.
- An unchanged Blueprint inspection snapshot was reportedly rejected by action discovery on Windows UE 5.8.0 with MCP 0.22.0. The inventory did not establish reproduction or resolution in 0.53.0.
- macOS lifecycle acceptance had an early editor exit before authenticated readiness, and its acceptance entry point was Windows-only. Native macOS lifecycle acceptance remained incomplete.
- macOS AI verification still had native, restart/socket, build-mode, and universal-packaging follow-ups.

The earlier implementation also lacked generic asset search and had inconsistent path scopes across tools. The new search requirements already address discovery and define explicit scopes; consistency with later inspection and authoring specifications still needs review.

## Additional capability backlog

Unimplemented or planned areas included MVVM, CommonUI/GAS authoring, PCG, dedicated spline operations, editor builds/project-file generation through MCP, PIE/runtime inspection and testing, Material/Niagara inspection and authoring, Animation Blueprint and Widget Animation authoring, screenshots/media retrieval, arbitrary Data Asset authoring, Curve Tables, import/export, Blueprint reparenting, and broader project settings.

These remain candidates for explicit scope decisions. The previous absence of a feature does not exclude it from the new project.
