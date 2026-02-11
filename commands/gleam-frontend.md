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
- `~/.claude/skills/gleam/references/frontend/lustre-gotchas.md`
- `~/.claude/skills/gleam/references/frontend/lustre-core.md`
- `~/.claude/skills/gleam/references/fundamentals/type-design.md`
- `~/.claude/skills/gleam/references/fundamentals/code-patterns.md`

**Load based on feature type:**

| Feature | Reference |
|---------|-----------|
| New page/route | `references/frontend/lustre-routing.md` |
| API/HTTP calls | `references/frontend/lustre-http.md` |
| Web components | `references/frontend/lustre-components.md` |
| Events/interactivity | `references/frontend/lustre-events.md` |
| UI composition | `references/frontend/lustre-ui-patterns.md` |
| Browser APIs | `references/frontend/lustre-browser-apis.md` |
| Effects/commands | `references/frontend/lustre-effects.md` |
| SSR/hydration | `references/frontend/lustre-advanced.md` |
| Parsing / DSLs | `references/fundamentals/parsing-nibble.md` |

All frontend references live under `~/.claude/skills/gleam/references/frontend/`.

### Step 3: Explore existing project structure

Before writing any code:

1. Read `gleam.toml` to understand dependencies and project config
2. Use Glob to find existing `.gleam` files in `src/`
3. Identify the project's conventions:
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
