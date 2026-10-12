# plugins — Tool integrations
> Last updated: 2026-10-12

Plugins extend Claude Code with tool integrations (LSP servers, MCP
tools, etc.). Each plugin is a directory with a `README.md` and
configuration files.

Inherits: top-level `CONTEXT.md`.

## Inventory
| Plugin       | Purpose                                              |
|--------------|------------------------------------------------------|
| `gleam-lsp`  | Gleam language server for code intelligence          |

## gleam-lsp
Spawns `gleam lsp` over stdio. Provides diagnostics, go-to-definition,
find-references, hover, completion, formatting, and code actions.

Requires the `gleam` binary in PATH. The server is registered by the
`lspServers` field of the `gleam-lsp` entry in
`.claude-plugin/marketplace.json` (`strict: false`). Without that field
Claude Code installs the plugin but starts no server. Bump the entry's
`version` on every change so installed caches refresh.
