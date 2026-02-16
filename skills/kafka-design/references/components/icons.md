---
title: Icon Sizes, Colors, and Accessibility
impact: MEDIUM
impactDescription: Consistent iconography with Lucide Icons and proper ARIA labels
tags: icons, lucide, sizes, colors, aria-label, accessibility
---

## Icons

Kafka uses **Lucide Icons** for consistency and clarity.

### Sizes

| Size | Pixels | Usage                          |
|------|--------|--------------------------------|
| `xs` | 12px   | Inline with caption text       |
| `sm` | 14px   | Inline with body text, badges  |
| `md` | 16px   | **Default**, buttons, fields   |
| `lg` | 20px   | Page headers, emphasis         |
| `xl` | 24px   | Modal headers, prominent icons |

### Colors

Always use semantic or text colors:

| Context         | Color             |
|-----------------|-------------------|
| Primary actions | `--indigo-400`    |
| Text inline     | `--text-secondary`|
| Success         | `--success-500`   |
| Warning         | `--warning-500`   |
| Error           | `--error-500`     |
| Disabled        | `--text-disabled` |

### Icon-Only Buttons

MUST have `aria-label` for accessibility:

```html
<button type="button" aria-label="Edit product"
  class="p-2 text-text-secondary hover:text-text-primary
         focus-visible:ring-3 focus-visible:ring-focus-ring">
  <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <path d="M11 4H4a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2v-7"/>
    <path d="M18.5 2.5a2.121 2.121 0 013 3L12 15l-4 1 1-4 9.5-9.5z"/>
  </svg>
</button>
```

### Resource

Lucide Icons: https://lucide.dev (MIT License)
