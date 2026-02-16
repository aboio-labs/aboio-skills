---
title: Dark Mode Color Philosophy and Implementation
impact: HIGH
impactDescription: Dark-mode-first design ensures comfortable extended use for publishing professionals
tags: colors, dark-mode, theme, oklch, philosophy
---

## Dark Mode First

Kafka is designed for **dark mode by default**. Light mode is available but not emphasized.

### Why Dark Mode First

1. **Extended use comfort:** Publishers work long hours reviewing catalogs and reports
2. **Focus on content:** Dark backgrounds reduce visual noise
3. **Editorial tradition:** Professional editing has historically used dark interfaces
4. **Energy efficiency:** OLED screens benefit from dark pixels

### Color Behavior Across Modes

**Anchor points (identical in both modes):**
- `--indigo-400`, `--indigo-500` (same in light and dark)
- `--violet-400`, `--violet-500`
- All semantic `*-400` and `*-500` values

**Divergent values (600+):**
- Light mode: 600+ gets darker (lower lightness)
- Dark mode: 600+ gets lighter (higher lightness)
- This ensures readable text on both backgrounds

### Theme Implementation

Use `data-theme` attribute on root element:

```css
:root[data-theme="dark"] {
  --canvas: oklch(0.13 0 0);
  --surface: oklch(0.165 0 0);
  --surface-raised: oklch(0.19 0 0);
  --text-primary: oklch(0.95 0 0);
  --text-secondary: oklch(0.70 0 0);
  --text-tertiary: oklch(0.55 0 0);
  --border: oklch(0.25 0 0);
  --hover: oklch(0.22 0 0);
  --active: oklch(0.26 0 0);
}

:root[data-theme="light"] {
  --canvas: oklch(0.985 0 0);
  --surface: oklch(0.97 0 0);
  --surface-raised: oklch(1.0 0 0);
  --text-primary: oklch(0.15 0 0);
  --text-secondary: oklch(0.45 0 0);
  --text-tertiary: oklch(0.60 0 0);
  --border: oklch(0.90 0 0);
  --hover: oklch(0.94 0 0);
  --active: oklch(0.90 0 0);
}
```

### Design Workflow

Always design in dark mode first, then adapt to light mode. When in doubt, test dark mode appearance before light mode.

### Shadow Adjustments

Dark mode shadows are heavier to maintain visible depth:

| Token        | Light Mode                    | Dark Mode                     |
|--------------|-------------------------------|-------------------------------|
| `--shadow-sm`  | 0 1px 2px rgba(0,0,0,0.05)   | 0 1px 3px rgba(0,0,0,0.4)    |
| `--shadow-md`  | 0 2px 8px rgba(0,0,0,0.08)   | 0 2px 12px rgba(0,0,0,0.5)   |
| `--shadow-lg`  | 0 4px 16px rgba(0,0,0,0.12)  | 0 4px 20px rgba(0,0,0,0.6)   |
| `--shadow-xl`  | 0 8px 32px rgba(0,0,0,0.16)  | 0 8px 40px rgba(0,0,0,0.7)   |
