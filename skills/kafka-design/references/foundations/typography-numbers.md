---
title: Numeric Typography (Tabular Numerals)
impact: MEDIUM
impactDescription: Aligned columns in tables and financial data
tags: typography, numbers, tabular-nums, font-features, currency
---

## Numeric Typography

Always use tabular numerals for columnar numeric data. This ensures digits align vertically in tables and financial reports.

### When to Use Tabular Numerals

- Prices in tables
- Quantities and stock levels
- Financial reports
- Any columnar numeric data

### CSS Implementation

```css
.numeric-column {
  font-feature-settings: "tnum";
  font-variant-numeric: tabular-nums;
}
```

### Utility Class

```css
.tabular-nums {
  font-feature-settings: "tnum";
  font-variant-numeric: tabular-nums;
}
```

### Currency Formatting

Always use 2 decimal places and locale-appropriate formatting:

| Currency | Format       | Example  |
|----------|-------------|----------|
| BRL      | R$ X.XXX,XX | R$ 89,90 |
| USD      | $X,XXX.XX   | $89.90   |
| EUR      | X.XXX,XX    | 89,90    |

See `patterns/currency-formatting.md` for full implementation with `Intl.NumberFormat`.

### Price Display HTML

```html
<span class="tabular-nums text-sm" data-price="89.90" data-currency="BRL">
  R$ 89,90
</span>
```

### ISBN and Code Display

Use the `mono` token (0.8125rem, weight 400) for ISBNs, order IDs, and other codes:

```html
<span class="font-mono text-sm text-text-secondary">978-3-16-148410-0</span>
```
