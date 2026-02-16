---
title: Modals and Dialogs (Focus Trap, Keyboard Navigation)
impact: HIGH
impactDescription: Focused task completion with full accessibility support
tags: modals, dialogs, focus-trap, keyboard, aria
---

## Modals & Dialogs

Interrupt workflow for focused tasks. Use sparingly. Always dismissible.

### Anatomy

```
+-------------------------------------+
|  [x]                                | <- Close button (top-right)
|                                     |
|  Modal Title                        | <- Header
|  Brief description of the action    |
|                                     |
|  ---------------------------------- |
|                                     |
|  Content area                       | <- Content (scrollable)
|                                     |
|  ---------------------------------- |
|                                     |
|  [Cancel]              [Confirm]    | <- Footer (actions right-aligned)
+-------------------------------------+
```

### Sizes

| Size   | Width  | Usage                              |
|--------|--------|------------------------------------|
| `sm`   | 400px  | Confirmations, alerts              |
| `md`   | 600px  | **Default**, simple forms          |
| `lg`   | 800px  | Complex forms, data review         |
| `xl`   | 1000px | Multi-step wizards, embedded tables|
| `full` | 90vw   | Full-screen editors (rare)         |

### Behavior

**Opening:**
- Fade in overlay (200ms ease)
- Scale up modal from 95% to 100% (200ms ease)
- Focus first interactive element
- Trap focus within modal
- Prevent body scroll

**Closing:**
- ESC key closes modal
- Click overlay closes (unless `disableOverlayClick`)
- Close button [x] always present
- Restore focus to trigger element

### Confirmation Modal HTML

```html
<div class="fixed inset-0 z-50 flex items-center justify-center p-4">
  <div class="fixed inset-0 bg-overlay" data-modal-overlay></div>
  <div class="relative w-full max-w-sm bg-surface-raised rounded-lg shadow-xl"
       role="dialog" aria-modal="true" aria-labelledby="modal-title">
    <div class="px-6 pt-6 pb-4">
      <button type="button"
        class="absolute top-4 right-4 text-text-tertiary hover:text-text-primary"
        data-modal-close aria-label="Close">
        <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M18 6L6 18M6 6l12 12"/>
        </svg>
      </button>
      <h2 id="modal-title" class="text-lg font-semibold text-text-primary">Delete Product?</h2>
      <p class="mt-2 text-sm text-text-secondary">
        This action cannot be undone. The product will be permanently removed.
      </p>
    </div>
    <div class="flex items-center justify-end gap-3 px-6 pb-6">
      <button type="button"
        class="h-10 px-4 border border-border rounded-md text-text-primary hover:bg-hover"
        data-modal-close>Cancel</button>
      <button type="button"
        class="h-10 px-4 bg-error-500 text-white rounded-md hover:bg-error-600">
        Delete Product</button>
    </div>
  </div>
</div>
```

### Focus Trap (JavaScript)

```javascript
class Modal {
  constructor(element) {
    this.element = element;
    this.overlay = element.querySelector('[data-modal-overlay]');
    this.closeButtons = element.querySelectorAll('[data-modal-close]');
    this.previousFocus = null;
    this.init();
  }

  init() {
    this.overlay?.addEventListener('click', () => this.close());
    this.closeButtons.forEach(btn => btn.addEventListener('click', () => this.close()));
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && this.isOpen()) this.close();
    });
    this.element.addEventListener('keydown', (e) => {
      if (e.key === 'Tab') this.handleTab(e);
    });
  }

  open() {
    this.previousFocus = document.activeElement;
    this.element.classList.remove('hidden');
    document.body.style.overflow = 'hidden';
    const first = this.element.querySelector(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
    first?.focus();
  }

  close() {
    this.element.classList.add('hidden');
    document.body.style.overflow = '';
    this.previousFocus?.focus();
  }

  isOpen() { return !this.element.classList.contains('hidden'); }

  handleTab(event) {
    const focusable = this.element.querySelectorAll(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
    const first = focusable[0];
    const last = focusable[focusable.length - 1];
    if (event.shiftKey && document.activeElement === first) {
      event.preventDefault(); last.focus();
    } else if (!event.shiftKey && document.activeElement === last) {
      event.preventDefault(); first.focus();
    }
  }
}
```
