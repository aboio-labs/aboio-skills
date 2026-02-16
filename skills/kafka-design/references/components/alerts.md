---
title: Alerts and Toast Notifications
impact: HIGH
impactDescription: Contextual feedback and event announcements
tags: alerts, toasts, notifications, auto-dismiss, aria-live
---

## Alerts & Notifications

### Alert Types

| Type   | Icon | Auto-dismiss | Usage                          |
|--------|------|-------------|--------------------------------|
| Inline | Yes  | No          | Contextual, page-level feedback|
| Toast  | Yes  | Yes         | Background task completion     |
| Banner | Opt  | No          | System-wide announcements      |

### Inline Alert HTML

```html
<div class="flex items-start gap-3 p-4 bg-success-muted border border-success-500/20 rounded-md"
     role="alert">
  <svg class="w-5 h-5 text-success-500 flex-shrink-0" viewBox="0 0 24 24"
       fill="none" stroke="currentColor" stroke-width="2">
    <path d="M22 11.08V12a10 10 0 11-5.93-9.14"/>
    <path d="M22 4L12 14.01l-3-3"/>
  </svg>
  <div class="flex-1">
    <p class="text-sm font-medium text-text-primary">Product created successfully</p>
    <p class="mt-1 text-sm text-text-secondary">The product is now available in your catalog.</p>
  </div>
  <button type="button" class="text-text-tertiary hover:text-text-primary" aria-label="Dismiss alert">
    <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M18 6L6 18M6 6l12 12"/>
    </svg>
  </button>
</div>
```

### Toast Notification System

**Position:** Top-right corner, 16px from edge
**Max Width:** 400px
**Stack:** Vertical, newest on top
**Animation:** Slide in from right, fade out

**Duration by type:**

| Type    | Duration   |
|---------|------------|
| Success | 3 seconds  |
| Info    | 5 seconds  |
| Warning | 8 seconds  |
| Error   | 12 seconds (or until dismissed) |

### Toast JavaScript

```javascript
function showToast(message, type = 'info', duration = 5000) {
  const toast = document.createElement('div');
  const colors = {
    success: 'bg-success-muted border-success-500/20 text-success-600',
    error: 'bg-error-muted border-error-500/20 text-error-600',
    warning: 'bg-warning-muted border-warning-500/20 text-warning-600',
    info: 'bg-info-muted border-info-500/20 text-info-600'
  };

  toast.className = `fixed top-4 right-4 max-w-sm p-4 ${colors[type]}
                     border rounded-md shadow-xl animate-slide-in`;
  toast.setAttribute('role', 'alert');
  toast.innerHTML = `
    <div class="flex items-start gap-3">
      <div class="flex-1 text-sm">${message}</div>
      <button type="button" class="text-text-tertiary hover:text-text-primary"
        onclick="this.parentElement.parentElement.remove()">
        <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M18 6L6 18M6 6l12 12"/>
        </svg>
      </button>
    </div>`;

  document.body.appendChild(toast);

  if (duration > 0) {
    setTimeout(() => {
      toast.style.opacity = '0';
      toast.style.transform = 'translateX(100%)';
      setTimeout(() => toast.remove(), 300);
    }, duration);
  }
}
```
