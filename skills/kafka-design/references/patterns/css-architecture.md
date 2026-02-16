---
title: CSS Architecture (Custom Properties, Utility-First)
impact: HIGH
impactDescription: Design token system with CSS custom properties and Tailwind
tags: css, architecture, custom-properties, tokens, tailwind
---

## CSS Architecture

Kafka uses **utility-first CSS** (Tailwind CSS) with custom design tokens defined as CSS custom properties.

### Design Token Declaration

```css
:root {
  /* Colors */
  --indigo-400: oklch(0.66 0.14 232);
  --violet-400: oklch(0.66 0.18 270);
  --success-500: oklch(0.58 0.16 145);
  --warning-500: oklch(0.68 0.18 70);
  --error-500: oklch(0.55 0.26 27);
  --info-500: oklch(0.58 0.18 240);

  /* Surfaces */
  --canvas: oklch(0.13 0 0);
  --surface: oklch(0.165 0 0);
  --surface-raised: oklch(0.19 0 0);

  /* Text */
  --text-primary: oklch(0.95 0 0);
  --text-secondary: oklch(0.70 0 0);
  --text-tertiary: oklch(0.55 0 0);

  /* Spacing */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-5: 1.25rem;
  --space-6: 1.5rem;
  --space-8: 2rem;

  /* Typography */
  --font-sans: "Iosevka Web", "Iosevka", ui-monospace, "SF Mono", Menlo, monospace;
  --font-size-body: 0.875rem;

  /* Shadows */
  --shadow-md: 0 2px 12px rgba(0,0,0,0.5);

  /* Radius */
  --radius-md: 6px;
}
```

### Token Categories

| Category | Prefix        | Example                    |
|----------|---------------|----------------------------|
| Colors   | `--indigo-*`  | `--indigo-400`            |
| Surfaces | `--canvas`    | `--surface-raised`        |
| Text     | `--text-*`    | `--text-primary`          |
| Borders  | `--border*`   | `--border-strong`         |
| Spacing  | `--space-*`   | `--space-4`               |
| Radius   | `--radius-*`  | `--radius-md`             |
| Shadows  | `--shadow-*`  | `--shadow-lg`             |

### Theme Switching

Use `data-theme` attribute (see `dark-mode.md`):

```css
:root[data-theme="dark"] { /* dark values */ }
:root[data-theme="light"] { /* light values */ }
```
