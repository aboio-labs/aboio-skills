---
title: Touch Target Sizes (44x44px Minimum)
impact: HIGH
impactDescription: WCAG 2.5.5 compliance for mobile and touch interfaces
tags: accessibility, touch, targets, mobile, wcag
---

## Touch Targets

### Minimum Sizes

| Category     | Size   | Usage                              |
|-------------|--------|------------------------------------|
| **Minimum** | 44x44px| WCAG 2.5.5 requirement            |
| Comfortable | 48x48px| Recommended default               |
| Small UI    | 40x40px| Acceptable if not primary action   |

### CSS

```css
/* Prevent double-tap zoom */
button { touch-action: manipulation; }
```

### Viewport Meta

Never disable pinch-zoom:

```html
<!-- Correct -->
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=yes">

<!-- FORBIDDEN -->
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
```

### Guidelines

- Small icon buttons (e.g., 32px visible) must have 44px touch area via padding
- Spacing between touch targets should be at least 8px
- Primary actions should use 48px targets
