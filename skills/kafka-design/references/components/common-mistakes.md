---
title: Common Component Mistakes
impact: HIGH
impactDescription: Avoid frequently seen errors in Kafka UI implementation
tags: mistakes, anti-patterns, checklist, accessibility, consistency
---

## Common Mistakes

### Color Mistakes

- Using hex values instead of OKLCH tokens
- Using light-mode colors in dark mode context
- Using indigo for non-interactive decorative elements
- Using violet for primary actions (indigo only)
- Using color alone to convey information (always add icon/text)

### Button Mistakes

- Missing `type="button"` (defaults to `type="submit"` in forms)
- Icon-only buttons without `aria-label`
- Multiple primary buttons in the same section
- Using `<div>` or `<span>` instead of `<button>`
- Custom button variants outside the 5 defined ones
- All caps button text (use sentence case)

### Form Mistakes

- Labels not associated with inputs (missing `for` attribute)
- Missing `aria-required="true"` on required fields
- Red label text for required fields (use asterisk only)
- Missing `autocomplete` attributes on common fields
- Error messages without `role="alert"`
- Removing focus outline without replacement

### Typography Mistakes

- Using font-weight 700 (Bold) or 300 (Light)
- Using proportional numerals in table columns
- Text exceeding 90 characters per line
- Body text smaller than 14px (0.875rem)
- All caps except for abbreviations and `overline` token

### Accessibility Mistakes

- Removing `:focus` outline without `:focus-visible` replacement
- Missing `role="dialog"` and `aria-modal="true"` on modals
- Focus not trapped inside modals
- Touch targets smaller than 44x44px
- Disabling pinch-zoom (`user-scalable=no`)
- Missing `prefers-reduced-motion` media query
- Using `<div onClick>` instead of `<button>`

### Layout Mistakes

- Mixing radius values on adjacent elements
- Shadows on non-floating elements
- Inconsistent card padding within same view
- Missing `data-theme` attribute for dark/light mode switching
