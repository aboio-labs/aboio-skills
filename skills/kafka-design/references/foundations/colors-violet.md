---
title: Violet Secondary Color Palette
impact: HIGH
impactDescription: Organizational identity color for badges, tags, and secondary branding
tags: colors, violet, secondary, org-identity, oklch
---

## Violet Secondary Palette

Violet (`hue: 270`) represents creativity and organizational identity. Used for org badges, tags, and secondary branding elements.

### Token Table

| Token          | OKLCH Light Mode        | OKLCH Dark Mode         | Hex (Light / Dark)     | Usage                |
|----------------|-------------------------|-------------------------|------------------------|----------------------|
| `--violet-50`  | oklch(0.97 0.01 270)    | oklch(0.18 0.02 270)    | #f5f3ff / #221e2e      | Org badge backgrounds|
| `--violet-100` | oklch(0.94 0.03 270)    | oklch(0.22 0.04 270)    | #ede9fe / #2b263a      | Hover on org elements|
| `--violet-200` | oklch(0.88 0.06 270)    | oklch(0.28 0.08 270)    | #ddd6fe / #3a3250      | Tag backgrounds      |
| `--violet-300` | oklch(0.78 0.10 270)    | oklch(0.36 0.10 270)    | #c4b5fd / #4f426b      | Border accents       |
| `--violet-400` | oklch(0.66 0.18 270)    | oklch(0.66 0.18 270)    | #a78bfa / #a78bfa      | **Org identity**     |
| `--violet-500` | oklch(0.58 0.20 270)    | oklch(0.58 0.20 270)    | #8b5cf6 / #8b5cf6      | Active org elements  |
| `--violet-600` | oklch(0.50 0.21 270)    | oklch(0.72 0.18 270)    | #7c3aed / #b89dff      | Hover on secondary   |
| `--violet-700` | oklch(0.42 0.18 270)    | oklch(0.78 0.14 270)    | #6d28d9 / #ceb5ff      | Emphasis             |
| `--violet-800` | oklch(0.33 0.14 270)    | oklch(0.84 0.10 270)    | #5b21b6 / #ddc9ff      | Strong emphasis      |
| `--violet-900` | oklch(0.25 0.10 270)    | oklch(0.90 0.06 270)    | #4c1d95 / #e9dcff      | Maximum emphasis     |

### Usage Patterns

**Organization badges:**
```css
.org-badge {
  background: oklch(0.66 0.18 270 / 10%); /* --violet-400/10% */
  color: var(--violet-400);
  border: 1px solid oklch(0.66 0.18 270 / 20%); /* --violet-400/20% */
}
```

**Category tags:**
```css
.category-tag {
  background: var(--violet-200);
  color: var(--violet-700);
}
```

### Guidelines

- Use sparingly; only when differentiating from primary indigo actions
- Never use violet for primary CTAs or main navigation
- Violet badges identify organizations, workspaces, or categories
- 400-500 values are identical in both modes (anchor point)
