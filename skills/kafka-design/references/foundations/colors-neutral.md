---
title: Neutral Color Palette (Surfaces, Text, Borders, Interactive)
impact: CRITICAL
impactDescription: Core achromatic palette for all text, backgrounds, borders, and interactive states
tags: colors, neutral, text, surface, border, achromatic, oklch
---

## Neutral Palette

Zero chroma (pure grayscale) for maximum flexibility. Avoids color temperature bias, ensuring accent colors stand out.

### Surface Colors

| Token               | OKLCH Light         | OKLCH Dark          | Hex (Light / Dark)    | Usage                |
|---------------------|---------------------|---------------------|-----------------------|----------------------|
| `--canvas`          | oklch(0.985 0 0)    | oklch(0.13 0 0)     | #fafafa / #101112     | **App background**   |
| `--surface`         | oklch(0.97 0 0)     | oklch(0.165 0 0)    | #f5f5f5 / #1a1b1e     | **Card backgrounds** |
| `--surface-raised`  | oklch(1.0 0 0)      | oklch(0.19 0 0)     | #ffffff / #232427     | **Elevated surfaces**|
| `--overlay`         | oklch(0 0 0 / 50%)  | oklch(0 0 0 / 70%)  | rgba(0,0,0,0.5/0.7)  | Modal overlays       |

**Elevation order:** Canvas → Surface → Surface-raised (creates depth).

### Text Colors

| Token               | OKLCH Light         | OKLCH Dark          | Hex (Light / Dark)    | Usage                |
|---------------------|---------------------|---------------------|-----------------------|----------------------|
| `--text-primary`    | oklch(0.15 0 0)     | oklch(0.95 0 0)     | #171717 / #f2f2f2     | **Primary body text**|
| `--text-secondary`  | oklch(0.45 0 0)     | oklch(0.70 0 0)     | #737373 / #b3b3b3     | **Secondary text**   |
| `--text-tertiary`   | oklch(0.60 0 0)     | oklch(0.55 0 0)     | #999999 / #8c8c8c     | **Hints, captions**  |
| `--text-disabled`   | oklch(0.72 0 0)     | oklch(0.40 0 0)     | #b8b8b8 / #666666     | Disabled text        |
| `--text-inverted`   | oklch(0.95 0 0)     | oklch(0.15 0 0)     | #f2f2f2 / #171717     | Text on dark/light bg|

### Border & Divider Colors

| Token               | OKLCH Light          | OKLCH Dark           | Hex (Light / Dark)  | Usage               |
|---------------------|----------------------|----------------------|---------------------|---------------------|
| `--border`          | oklch(0.90 0 0)      | oklch(0.25 0 0)      | #e6e6e6 / #404040   | **Default borders** |
| `--border-strong`   | oklch(0.82 0 0)      | oklch(0.32 0 0)      | #d1d1d1 / #525252   | Emphasized borders  |
| `--divider`         | oklch(0.94 0 0)      | oklch(0.20 0 0)      | #f0f0f0 / #333333   | Section dividers    |

### Interactive States

| Token               | OKLCH Light                     | OKLCH Dark                      | Hex (Light / Dark)  | Usage               |
|---------------------|---------------------------------|---------------------------------|---------------------|---------------------|
| `--hover`           | oklch(0.94 0 0)                 | oklch(0.22 0 0)                 | #f0f0f0 / #383838   | Hover backgrounds   |
| `--active`          | oklch(0.90 0 0)                 | oklch(0.26 0 0)                 | #e6e6e6 / #424242   | Active/pressed      |
| `--selected`        | oklch(0.66 0.14 232 / 10%)     | oklch(0.66 0.14 232 / 15%)     | Indigo @ 10-15%     | Selected items      |
| `--focus-ring`      | oklch(0.66 0.14 232)           | oklch(0.66 0.14 232)           | #818cf8             | Focus indicator     |

**Note:** `--selected` and `--focus-ring` use indigo, not neutral gray.
