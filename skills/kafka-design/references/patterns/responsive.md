---
title: Responsive Strategy and Breakpoints
impact: MEDIUM
impactDescription: Adaptive layout behavior across device sizes
tags: responsive, breakpoints, mobile, tablet, desktop
---

## Responsive Strategy

### Breakpoints

| Breakpoint | Width    | Behavior                                    |
|------------|----------|---------------------------------------------|
| Mobile     | <768px   | Single column, card list instead of tables  |
| Tablet     | 768-1024px| Two columns, horizontal scroll for tables  |
| Desktop    | 1024px+  | Full three-column layout                    |
| Wide       | 1440px+  | Optimal for page content (1200px max-width) |

### Table Responsive Behavior

**Mobile (<768px):**
- Convert to card list view
- Show 3-4 most important columns
- Rest available in detail view
- Actions accessible via swipe or tap

**Tablet (768-1024px):**
- Horizontal scroll with sticky first column
- Visible scroll indicator
- Actions column always visible

### Three-Column Manager Collapse

- **<1024px:** Hide categories column, show as dropdown
- **<768px:** Full-width list view, detail as separate page/overlay

### Content Width

Always constrain page content to `max-width: 1200px` with `margin-inline: auto`.
