---
title: Motion and Animation Guidelines
impact: MEDIUM
impactDescription: Respect prefers-reduced-motion and use hardware-accelerated properties
tags: accessibility, motion, animation, reduced-motion, performance
---

## Motion & Animation

### Reduced Motion

Always respect `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Animation Rules

**Only animate:** `transform` and `opacity` (hardware accelerated)

**Never animate:** `height`, `width`, `top`, `left` (causes reflow)

### Duration Guidelines

| Context          | Duration |
|------------------|----------|
| Most transitions | 150-200ms|
| Complex animations| 300ms   |
| Easing (entry)   | `ease` or `ease-out` |
| Easing (exit)    | `ease-in`|

### Example

```css
.card-interactive {
  transition: box-shadow 150ms ease, border-color 150ms ease;
}

.toast-enter {
  animation: slide-in 200ms ease-out;
}

@keyframes slide-in {
  from { transform: translateX(100%); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}
```
