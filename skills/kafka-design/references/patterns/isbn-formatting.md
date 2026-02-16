---
title: ISBN Formatting (978-X-XX-XXXXXX-X)
impact: HIGH
impactDescription: Standard ISBN display for publishing interfaces
tags: publishing, isbn, formatting, monospace
---

## ISBN Display

Always format ISBNs with hyphens for readability.

### Formats

- **ISBN-13:** `978-3-16-148410-0`
- **ISBN-10:** `0-306-40615-2`

### HTML

```html
<span class="font-mono text-sm text-text-secondary" data-isbn="9783161484100">
  978-3-16-148410-0
</span>
```

### JavaScript Formatter

```javascript
function formatISBN(isbn) {
  const clean = isbn.replace(/[-\s]/g, '');

  if (clean.length === 13) {
    return `${clean.slice(0,3)}-${clean.slice(3,4)}-${clean.slice(4,6)}-${clean.slice(6,12)}-${clean.slice(12)}`;
  }

  if (clean.length === 10) {
    return `${clean.slice(0,1)}-${clean.slice(1,4)}-${clean.slice(4,9)}-${clean.slice(9)}`;
  }

  return isbn;
}

// Apply to all ISBN elements
document.querySelectorAll('[data-isbn]').forEach(el => {
  el.textContent = formatISBN(el.dataset.isbn);
});
```

### Display Rules

- Always use `font-mono` (Iosevka) for ISBNs
- Use `text-text-secondary` color
- Store raw ISBN in `data-isbn` attribute for programmatic access
- Size: `body-sm` (0.8125rem) or `body` (0.875rem)
