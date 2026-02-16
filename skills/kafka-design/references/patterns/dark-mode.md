---
title: Dark Mode Implementation
impact: HIGH
impactDescription: data-theme attribute system for dark/light mode switching
tags: dark-mode, theme, implementation, css, data-theme
---

## Dark Mode Implementation

Use `data-theme` attribute on root element. Dark mode is the default.

### HTML Setup

```html
<html data-theme="dark">
```

### CSS Structure

```css
:root[data-theme="dark"] {
  --canvas: oklch(0.13 0 0);
  --surface: oklch(0.165 0 0);
  --surface-raised: oklch(0.19 0 0);
  --overlay: oklch(0 0 0 / 70%);

  --text-primary: oklch(0.95 0 0);
  --text-secondary: oklch(0.70 0 0);
  --text-tertiary: oklch(0.55 0 0);
  --text-disabled: oklch(0.40 0 0);

  --border: oklch(0.25 0 0);
  --border-strong: oklch(0.32 0 0);
  --divider: oklch(0.20 0 0);

  --hover: oklch(0.22 0 0);
  --active: oklch(0.26 0 0);

  --shadow-md: 0 2px 12px rgba(0,0,0,0.5);
  --shadow-lg: 0 4px 20px rgba(0,0,0,0.6);
}

:root[data-theme="light"] {
  --canvas: oklch(0.985 0 0);
  --surface: oklch(0.97 0 0);
  --surface-raised: oklch(1.0 0 0);
  --overlay: oklch(0 0 0 / 50%);

  --text-primary: oklch(0.15 0 0);
  --text-secondary: oklch(0.45 0 0);
  --text-tertiary: oklch(0.60 0 0);
  --text-disabled: oklch(0.72 0 0);

  --border: oklch(0.90 0 0);
  --border-strong: oklch(0.82 0 0);
  --divider: oklch(0.94 0 0);

  --hover: oklch(0.94 0 0);
  --active: oklch(0.90 0 0);

  --shadow-md: 0 2px 8px rgba(0,0,0,0.08);
  --shadow-lg: 0 4px 16px rgba(0,0,0,0.12);
}
```

### Theme Toggle (JavaScript)

```javascript
function toggleTheme() {
  const current = document.documentElement.dataset.theme;
  document.documentElement.dataset.theme = current === 'dark' ? 'light' : 'dark';
  localStorage.setItem('theme', document.documentElement.dataset.theme);
}

// Initialize from stored preference
const stored = localStorage.getItem('theme');
if (stored) document.documentElement.dataset.theme = stored;
```

### Design Workflow

Always design in dark mode first, then adapt to light mode.
