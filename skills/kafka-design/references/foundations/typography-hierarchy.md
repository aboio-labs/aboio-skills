---
title: Typography Hierarchy Rules
impact: HIGH
impactDescription: Consistent visual structure through size, weight, and color
tags: typography, hierarchy, headings, text-color
---

## Typography Hierarchy

Create hierarchy through size, weight, and color. Never use multiple font families.

### Hierarchy Chain

```
Display (2rem, 600, text-primary)
  |
Title (1.5rem, 600, text-primary)
  |
Subtitle (1.25rem, 600, text-primary)
  |
Body (0.875rem, 400, text-primary)
  |
Secondary Body (0.875rem, 400, text-secondary)
  |
Caption (0.75rem, 400, text-tertiary)
```

### Weight Rules

| Weight | Token    | Where to Use                                     |
|--------|----------|--------------------------------------------------|
| 400    | Regular  | All body text, descriptions, secondary info      |
| 500    | Medium   | Labels, button text, table headers, form labels  |
| 600    | Semibold | Headings (display, title, subtitle), key metrics |

### Color-Based Hierarchy

Use `--text-primary`, `--text-secondary`, and `--text-tertiary` to create depth without changing size:

```html
<div>
  <p class="text-sm font-semibold text-text-primary">Order #12345</p>
  <p class="text-sm text-text-secondary">3 items, shipped via DHL</p>
  <p class="text-xs text-text-tertiary">Updated 2 hours ago</p>
</div>
```

### Don't

- Never use 300 (Light), 700 (Bold), or 800+ (Extra Bold)
- Never use multiple font families for hierarchy
- Never use all caps except for `overline` token and abbreviations (ISBN, NF-e)
- Never use underline for emphasis (reserve for links)
