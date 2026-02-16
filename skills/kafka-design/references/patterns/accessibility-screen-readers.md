---
title: Screen Reader Support (ARIA, Semantic HTML)
impact: CRITICAL
impactDescription: Proper ARIA labels, live regions, and semantic structure
tags: accessibility, screen-reader, aria, live-regions, semantic-html
---

## Screen Reader Support

### ARIA Labels

- All icon-only buttons have `aria-label`
- Form fields have associated labels (explicit `for` attribute)
- Complex widgets use `aria-describedby` for instructions
- Live regions announce dynamic content changes

### ARIA Live Regions

```html
<!-- Toast notifications -->
<div role="alert" aria-live="polite" aria-atomic="true" class="sr-only">
  <!-- Message announced to screen readers -->
</div>

<!-- Loading states -->
<div role="status" aria-live="polite" aria-busy="true">
  <svg class="animate-spin w-5 h-5" viewBox="0 0 24 24">
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/>
    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/>
  </svg>
  <span class="ml-2">Loading products...</span>
</div>
```

### Dynamic Announcements (JavaScript)

```javascript
function announceToScreenReader(message) {
  const el = document.createElement('div');
  el.setAttribute('role', 'alert');
  el.setAttribute('aria-live', 'polite');
  el.setAttribute('aria-atomic', 'true');
  el.className = 'sr-only';
  el.textContent = message;
  document.body.appendChild(el);
  setTimeout(() => el.remove(), 1000);
}
```

### Semantic HTML Checklist

- `<button>` for actions, `<a>` for navigation
- `<nav>` wraps navigation, `<main>` wraps primary content
- `<article>` for independent content blocks
- `role="dialog"` and `aria-modal="true"` on modals
- `aria-labelledby` points to modal title heading
