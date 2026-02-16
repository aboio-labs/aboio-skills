---
title: Keyboard Navigation and Tab Order
impact: CRITICAL
impactDescription: Full keyboard accessibility for all interactive elements
tags: accessibility, keyboard, tab-order, shortcuts, skip-links
---

## Keyboard Navigation

### Tab Order

- All interactive elements are keyboard accessible
- Tab order follows visual hierarchy (left-to-right, top-to-bottom)
- Skip links allow jumping to main content
- Focus indicator always visible (3px ring, `--focus-ring`)

### Keyboard Shortcuts

| Action         | Shortcut      | Context              |
|----------------|---------------|----------------------|
| Search         | `/` or `Cmd+K`| Global               |
| Save           | `Cmd+S`       | Forms, editors       |
| Close modal    | `Esc`         | Modals, dialogs      |
| Navigate table | Arrow keys    | Data tables          |
| Select all     | `Cmd+A`       | Tables with checkboxes|

### Skip Link

```html
<a href="#main-content" class="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4
   focus:z-50 focus:bg-surface-raised focus:px-4 focus:py-2 focus:rounded-md focus:shadow-lg">
  Skip to main content
</a>
```

### Semantic HTML Requirements

- `<button>` for buttons (never `<div>` with `onClick`)
- `<a>` for navigation links
- `<nav>` for navigation sections
- `<main>` for primary content
- `<article>` for independent content blocks
