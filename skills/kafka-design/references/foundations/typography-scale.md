---
title: Type Scale (Modular 1.25 Ratio)
impact: HIGH
impactDescription: Hierarchical text sizing for consistent visual structure
tags: typography, type-scale, font-size, line-height, letter-spacing
---

## Type Scale

Kafka uses a **modular scale** with a 1.25 ratio for hierarchical clarity.

### Scale Table

| Token      | Size      | Weight | Line Height | Letter Spacing | Usage                            |
|------------|-----------|--------|-------------|----------------|----------------------------------|
| `display`  | 2rem      | 600    | 1.2         | -0.02em        | Page titles (Dashboard, Orders)  |
| `title`    | 1.5rem    | 600    | 1.25        | -0.015em       | Section headers (Recent Orders)  |
| `subtitle` | 1.25rem   | 600    | 1.3         | -0.01em        | Card titles (Order #12345)       |
| `body-lg`  | 1rem      | 400    | 1.5         | 0              | Emphasized body text             |
| `body`     | 0.875rem  | 400    | 1.5         | 0              | **Primary body text**            |
| `body-sm`  | 0.8125rem | 400    | 1.5         | 0              | Dense tables, metadata           |
| `caption`  | 0.75rem   | 400    | 1.4         | 0              | Hints, timestamps, labels        |
| `overline` | 0.6875rem | 500    | 1.2         | 0.05em         | Category labels (uppercase)      |
| `mono`     | 0.8125rem | 400    | 1.4         | 0              | ISBNs, IDs, code                 |

### CSS Custom Properties

```css
:root {
  --text-display: 600 2rem/1.2 var(--font-sans);
  --text-title: 600 1.5rem/1.25 var(--font-sans);
  --text-subtitle: 600 1.25rem/1.3 var(--font-sans);
  --text-body-lg: 400 1rem/1.5 var(--font-sans);
  --text-body: 400 0.875rem/1.5 var(--font-sans);
  --text-body-sm: 400 0.8125rem/1.5 var(--font-sans);
  --text-caption: 400 0.75rem/1.4 var(--font-sans);
  --text-overline: 500 0.6875rem/1.2 var(--font-sans);
  --text-mono: 400 0.8125rem/1.4 var(--font-mono);

  --ls-display: -0.02em;
  --ls-title: -0.015em;
  --ls-subtitle: -0.01em;
  --ls-overline: 0.05em;
}
```

### Optical Alignment

Use negative letter-spacing on headings for optical balance:
- Display: `-0.02em`
- Title: `-0.015em`
- Subtitle: `-0.01em`

### Accessibility Minimums

- **Minimum text size:** 12px (0.75rem) for captions only
- **Body text minimum:** 14px (0.875rem) for comfortable reading
- **Line height:** Minimum 1.5 for body text (WCAG SC 1.4.8)
- **Paragraph spacing:** At least 1.5x font size (WCAG SC 1.4.12)
