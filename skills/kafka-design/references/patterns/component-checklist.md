---
title: Component Checklist (Before Shipping)
impact: HIGH
impactDescription: Pre-ship verification for accessibility, states, and consistency
tags: checklist, qa, shipping, accessibility, states
---

## Component Checklist

Before shipping any component, verify all items:

### Interaction

- [ ] All interactive elements have `type` attribute
- [ ] Icon-only buttons have `aria-label`
- [ ] Keyboard navigation works correctly

### Accessibility

- [ ] Form fields have associated labels
- [ ] Focus states use `:focus-visible`, not `:focus`
- [ ] Color contrast meets WCAG AA (4.5:1 minimum)
- [ ] Touch targets are 44x44px minimum
- [ ] Screen reader announces state changes
- [ ] Error states have `aria-invalid` and `aria-describedby`

### Animation

- [ ] Animations respect `prefers-reduced-motion`
- [ ] Only `transform` and `opacity` are animated

### Consistency

- [ ] Uses design tokens (not hardcoded colors/sizes)
- [ ] Border radius matches context (`--radius-md` default)
- [ ] Shadows follow elevation hierarchy
- [ ] Typography uses only weights 400/500/600
- [ ] Spacing uses 4px grid values
