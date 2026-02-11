# Gleam LSP

Gleam language server for code intelligence in Claude Code.

## What This Plugin Provides

This plugin enables Claude Code to use the official Gleam language server (`gleam lsp`), providing:

- **Diagnostics:** Real-time error detection and warnings
- **Go to Definition:** Navigate to function, type, and variable definitions
- **Find References:** Locate all usages of a symbol
- **Hover Information:** View type signatures and documentation
- **Document Symbols:** Navigate file structure
- **Code Formatting:** Format code using Gleam's official formatter
- **Code Actions:** Quick fixes and refactorings
- **Completion:** Type-aware code completion

## Supported File Extensions

- `.gleam`

## Requirements

This plugin requires the Gleam binary to be installed and available in your PATH.

### Installation

**macOS:**
```bash
brew install gleam
```

**Linux:**
```bash
curl -sSfL https://gleam.run/install.sh | sh
```

**Windows:**
```powershell
scoop install gleam
```

**Verify:**
```bash
gleam --version
```

## How It Works

The plugin spawns the Gleam language server using the command:
```bash
gleam lsp
```

Communication happens over stdio using the Language Server Protocol (LSP).

## Configuration

The LSP server is configured with:

```json
{
  "command": "gleam",
  "args": ["lsp"],
  "extensionToLanguage": {
    ".gleam": "gleam"
  }
}
```

## Installation in Claude Code

Install this plugin from the Aboio marketplace:

```bash
claude plugin install gleam-lsp@aboio-agent-skills
```

Or add the Aboio marketplace and install:

```bash
claude marketplace add https://github.com/aboio/agent-skills
claude plugin install gleam-lsp
```

## Usage in Claude Code

Once installed, the LSP automatically starts when Claude opens a `.gleam` file in a Gleam project (directory containing `gleam.toml`).

Claude can then use LSP features like:

```
User: "Find where the User type is defined"
Claude: *uses lsp.goToDefinition*

User: "Show me all places where parse_request is used"
Claude: *uses lsp.findReferences*

User: "Are there any errors in this file?"
Claude: *uses lsp.diagnostics*
```

## Troubleshooting

### LSP Server Not Starting

Ensure you're in a Gleam project directory (contains `gleam.toml`):
```bash
ls gleam.toml
```

### Command Not Found

Verify Gleam is in your PATH:
```bash
which gleam
gleam --version
```

### Test LSP Manually

```bash
cd your-gleam-project
gleam lsp
# Should output: Gleam Language Server listening on stdio
```

## Resources

- [Gleam Language Server Documentation](https://gleam.run/language-server/)
- [Gleam Official Site](https://gleam.run/)
- [Language Server Protocol](https://microsoft.github.io/language-server-protocol/)
- [Aboio Agent Skills Repository](https://github.com/aboio/agent-skills)
