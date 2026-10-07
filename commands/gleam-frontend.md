---
description: Build a Gleam/Lustre frontend feature (page, route, component, HTTP)
argument-hint: <feature-description>
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit]
---

# Gleam Frontend Feature Builder

Build a frontend feature following Gleam/Lustre best practices — pages, routes, components, HTTP integration, and more.

## Instructions

### Step 1: Understand the request

The user wants to build: **$ARGUMENTS**

Parse what kind of frontend feature this involves. It may include one or more of:
- New page/route
- UI component
- API/HTTP integration
- Browser API usage
- Web component
- Event handling/interactivity

### Step 2: Load relevant references

Based on the feature type, read the appropriate reference files:

**Always load (gotchas and core):**
- `${CLAUDE_PLUGIN_ROOT}/skills/lustre/references/lustre-gotchas.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/lustre/references/lustre-core.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/gleam/references/fundamentals/type-design.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/gleam/references/fundamentals/code-patterns.md`

**Load based on feature type:**

| Feature | Reference |
|---------|-----------|
| New page/route | `skills/lustre/references/lustre-routing.md` |
| API/HTTP calls | `skills/lustre/references/lustre-http.md` |
| Web components | `skills/lustre/references/lustre-components.md` |
| Events/interactivity | `skills/lustre/references/lustre-events.md` |
| UI composition | `skills/lustre/references/lustre-ui-patterns.md` |
| Browser APIs | `skills/lustre/references/lustre-browser-apis.md` |
| Effects/commands | `skills/lustre/references/lustre-effects.md` |
| SSR/hydration | `skills/lustre/references/lustre-advanced.md` |
| Parsing / DSLs | `skills/gleam/references/fundamentals/parsing-nibble.md` |

Table paths are relative to the plugin root, `${CLAUDE_PLUGIN_ROOT}`.

### Step 3: Explore existing project structure

**Follow `references/token-efficiency.md` rules.** Explore surgically, not exhaustively.

Before writing any code:

1. Grep `gleam.toml` for specific dependencies you need (e.g., `lustre`, `modem`, `rsvp`)
2. Use Glob to find existing `.gleam` files matching your feature area (not all of `src/`)
3. Identify the project's conventions using targeted Grep queries:
   - How the app is structured (single module vs multi-page)
   - Model/Msg type organization
   - Where views are defined
   - Routing patterns (if using modem)
   - How HTTP requests are handled (if using rsvp)
   - CSS/styling approach

### Step 4: Implement the feature

Follow the patterns from the loaded references:

- Use the MVU (Model-View-Update) architecture
- Define Msg variants for all user interactions
- Use effects for side effects (HTTP, browser APIs, timers)
- Keep view functions pure — no side effects
- Use `lustre/element/html` for HTML elements
- Use `lustre/attribute` and `lustre/event` for attributes and handlers
- Follow the project's existing component/view patterns

### Step 5: Verify

After implementation:

1. Run `gleam check` to verify the code compiles
2. Run `gleam format` to ensure consistent formatting
3. If the project has tests, run `gleam test`

## Arguments

The user invoked this command with: $ARGUMENTS
