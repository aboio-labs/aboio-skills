---
title: Focus States and Focus Management
impact: CRITICAL
impactDescription: Visible focus indicators for all interactive elements
tags: accessibility, focus, focus-visible, focus-trap, outline
---

## Focus States

### Focus Indicator Style

```css
.focusable:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px var(--focus-ring);
  border-color: var(--indigo-400);
}
```

**FORBIDDEN:** Never remove focus without replacement:

```css
/* NEVER do this */
*:focus { outline: none; }
```

### Focus-Visible vs Focus

Always use `:focus-visible` (not `:focus`) to show focus rings only for keyboard navigation, not mouse clicks.

### Focus Trapping in Modals

Modals and popovers must trap focus:

1. On open: move focus to first interactive element
2. Tab cycles through modal elements only
3. Shift+Tab cycles backward
4. On close: return focus to trigger element

See `components/modals.md` for full focus trap JavaScript implementation.

### Focus Ring Specs

- **Width:** 3px
- **Color:** `--focus-ring` (indigo-400)
- **Offset:** 2px from element edge
- **Contrast:** Minimum 3:1 against adjacent colors
