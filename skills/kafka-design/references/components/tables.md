---
title: Data Tables (Sorting, Responsive, Column Types)
impact: CRITICAL
impactDescription: Information-dense interface for catalog, order, and financial management
tags: tables, data, sorting, responsive, columns, pagination
---

## Data Tables

Information-dense interfaces requiring careful design for scannability and usability.

### Anatomy

```
+--------------------------------------------------+
| [Search]  [Filter]  [Filter]     [Bulk Actions]  | <- Toolbar
+--------------------------------------------------+
| [ ] Header 1  v   Header 2    Header 3    [...]  | <- Header row
+--------------------------------------------------+
| [x] Value 1       Value 2     Value 3      [...]  | <- Data rows
| [ ] Value 1       Value 2     Value 3      [...]  |
+--------------------------------------------------+
| Showing 1-10 of 247            [<- 1 2 3 ... ->]  | <- Footer
+--------------------------------------------------+
```

### Specifications

| Element      | Height | Padding           | Border                        |
|--------------|--------|-------------------|-------------------------------|
| Header row   | 44px   | 12px H, 16px V    | Bottom: 1px `--border-strong` |
| Data row     | 52px   | 12px H, 16px V    | Bottom: 1px `--border`        |
| Dense row    | 40px   | 8px H, 12px V     | Bottom: 1px `--border`        |

### Column Types

| Type     | Alignment | Font              | Additional                    |
|----------|-----------|-------------------|-------------------------------|
| Text     | Left      | Body (0.875rem)   | Truncate with ellipsis        |
| Numeric  | Right     | Body, tabular-nums| No truncation, fixed width    |
| Currency | Right     | Body, tabular-nums| Always 2 decimals             |
| Date     | Left      | Body              | Consistent format             |
| Status   | Left      | Badge (sm)        | Color-coded badges            |
| Actions  | Center    | Icon buttons      | Max 3 visible, rest in menu   |

### Row States

| State    | Background    | Border             | Additional          |
|----------|---------------|--------------------|--------------------|
| Default  | Transparent   | `--border`         | -                  |
| Hover    | `--hover`     | `--border`         | Highlight entire row|
| Selected | `--selected`  | `--indigo-400/30%` | Checkbox checked   |
| Active   | `--active`    | `--indigo-400`     | Currently focused  |

### Sorting

- Sortable columns show sort icon on hover
- Active sort shows filled icon (up/down)
- Click: ascending -> descending -> clear
- Single column sort only (no multi-column)

### Responsive Behavior

**Mobile (<768px):** Convert to card list view. Show 3-4 most important columns.

**Tablet (768-1024px):** Horizontal scroll with sticky first column. Visible scroll indicator.
