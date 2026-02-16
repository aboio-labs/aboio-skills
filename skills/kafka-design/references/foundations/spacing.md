---
title: Spacing Scale (4px Base Unit)
impact: HIGH
impactDescription: Predictable, harmonious spacing across all interfaces
tags: spacing, layout, gap, padding, margin
---

## Spacing Scale

Kafka uses a **4px base unit** (0.25rem) for predictable, harmonious spacing.

### Scale Table

| Token  | Value     | Pixels | Usage Examples                           |
|--------|-----------|--------|------------------------------------------|
| `1`    | 0.25rem   | 4px    | Icon-to-text gap, tight badge padding    |
| `2`    | 0.5rem    | 8px    | Inline element gaps, small icon buttons  |
| `3`    | 0.75rem   | 12px   | Form label-to-input gap, button padding  |
| `4`    | 1rem      | 16px   | **Default gap**, paragraph spacing       |
| `5`    | 1.25rem   | 20px   | Card padding (horizontal & vertical)     |
| `6`    | 1.5rem    | 24px   | Section spacing within cards             |
| `8`    | 2rem      | 32px   | Large card padding, modal padding        |
| `10`   | 2.5rem    | 40px   | Page content margins                     |
| `12`   | 3rem      | 48px   | Section dividers, large gaps             |
| `16`   | 4rem      | 64px   | Major section spacing                    |
| `20`   | 5rem      | 80px   | Hero spacing, landing page gaps          |

### Common Applications

| Context                | Spacing         |
|------------------------|-----------------|
| Default card padding   | 20px (space-5)  |
| Default gap (cards)    | 16px (space-4)  |
| Sidebar padding        | 12px (space-3)  |
| Table cell (vertical)  | 12px            |
| Table cell (horizontal)| 16px            |
| Category item padding  | 8px (space-2)   |
| List item padding      | 12px (space-3)  |
| Modal body padding     | 32px (space-8)  |
| Page content margins   | 40px (space-10) |

### CSS Custom Properties

```css
:root {
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-5: 1.25rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-10: 2.5rem;
  --space-12: 3rem;
  --space-16: 4rem;
  --space-20: 5rem;
}
```

### Information Density Principle

Kafka prioritizes **actionable visibility**. Users should see relevant data without excessive scrolling. Use the tighter spacing values (space-2 through space-4) for dense data displays like tables and lists.
