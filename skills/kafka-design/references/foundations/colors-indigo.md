---
title: Indigo Primary Color Palette
impact: CRITICAL
impactDescription: Primary action color used for all buttons, links, and interactive elements
tags: colors, indigo, primary, actions, oklch
---

## Indigo Primary Palette

Indigo (`hue: 232`) represents authority, trust, and professionalism. It is the canonical color for all primary actions.

**Primary Action Color:** `--indigo-400` is the single source of truth for buttons, links, and CTAs.

### Token Table

| Token          | OKLCH Light Mode        | OKLCH Dark Mode         | Hex (Light / Dark)     | Usage                |
|----------------|-------------------------|-------------------------|------------------------|----------------------|
| `--indigo-50`  | oklch(0.97 0.01 232)    | oklch(0.18 0.02 232)    | #eef2ff / #1e1b3a      | Subtle backgrounds   |
| `--indigo-100` | oklch(0.94 0.02 232)    | oklch(0.22 0.04 232)    | #e0e7ff / #2a2645      | Hover states         |
| `--indigo-200` | oklch(0.88 0.04 232)    | oklch(0.28 0.06 232)    | #c7d2fe / #393458      | Active states        |
| `--indigo-300` | oklch(0.78 0.08 232)    | oklch(0.36 0.08 232)    | #a5b4fc / #4d4670      | Borders, dividers    |
| `--indigo-400` | oklch(0.66 0.14 232)    | oklch(0.66 0.14 232)    | #818cf8 / #818cf8      | **Primary actions**  |
| `--indigo-500` | oklch(0.58 0.18 232)    | oklch(0.58 0.18 232)    | #6366f1 / #6366f1      | Hover on primary     |
| `--indigo-600` | oklch(0.50 0.19 232)    | oklch(0.72 0.16 232)    | #4f46e5 / #a5b0ff      | Active on primary    |
| `--indigo-700` | oklch(0.42 0.16 232)    | oklch(0.78 0.12 232)    | #4338ca / #c0c8ff      | Text emphasis        |
| `--indigo-800` | oklch(0.33 0.12 232)    | oklch(0.84 0.08 232)    | #3730a3 / #d7dcff      | Strong emphasis      |
| `--indigo-900` | oklch(0.25 0.08 232)    | oklch(0.90 0.04 232)    | #312e81 / #e8ebff      | Maximum emphasis     |

### CSS Custom Properties

```css
:root {
  --indigo-400: oklch(0.66 0.14 232);
  --indigo-500: oklch(0.58 0.18 232);
  --indigo-600: oklch(0.50 0.19 232);
}

:root[data-theme="dark"] {
  --indigo-600: oklch(0.72 0.16 232);
  --indigo-700: oklch(0.78 0.12 232);
  --indigo-800: oklch(0.84 0.08 232);
  --indigo-900: oklch(0.90 0.04 232);
}
```

### Usage Rules

- **Buttons:** Background `--indigo-400`, hover `--indigo-500`, active `--indigo-600`
- **Links:** Text `--indigo-400`
- **Focus ring:** `--indigo-400` at 20-30% opacity
- **Selected items:** `--indigo-400` at 10-15% opacity
- 400-500 values are identical in both modes (anchor point)
- 600+ values diverge: darker in light mode, lighter in dark mode
