---
title: Three-Column Manager Pattern
impact: HIGH
impactDescription: Standard layout for entity management (Products, Orders, Consignments)
tags: layout, three-column, manager, sidebar, detail
---

## Three-Column Manager Pattern

For entity management screens (Products, Orders, Consignments).

### Structure

```
+-------------+---------------+--------------------------+
|  Categories |  Items List   |   Detail Editor          |
|   200px     |    280px      |   flex-1 (min 400px)     |
|   fixed     |    fixed      |   flexible               |
+-------------+---------------+--------------------------+
```

### Spacing

| Element              | Value                |
|----------------------|----------------------|
| Gap between columns  | 1px border (no gap)  |
| Internal padding     | 16px (space-4)       |
| Category item padding| 8px (space-2)        |
| List item padding    | 12px (space-3)       |

### Column Behavior

- **Categories (200px fixed):** Scrollable list of categories/filters. Compact padding.
- **Items List (280px fixed):** Scrollable item list. Selected item highlighted with `--selected`.
- **Detail Editor (flex-1, min 400px):** Full detail view. Uses available remaining space.

### Responsive Collapse

- Below 1024px: Hide categories column, show as dropdown
- Below 768px: Full-width list view, detail as separate page/overlay
