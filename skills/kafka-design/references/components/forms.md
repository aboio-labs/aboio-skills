---
title: Form Fields, Validation, and Accessibility
impact: CRITICAL
impactDescription: Data entry clarity with real-time validation and WCAG compliance
tags: forms, validation, inputs, accessibility, aria
---

## Form Fields

### Input Anatomy

```
[Label] *                    <- Label (font-weight: 500, text-secondary)
+-------------------------+
| Placeholder text        |  <- Input (h-10, px-3, rounded-md)
+-------------------------+
  Hint text or char count    <- Caption (0.75rem, text-tertiary)
```

### Field States

| State     | Border          | Background       | Ring                  | Additional                |
|-----------|-----------------|------------------|-----------------------|---------------------------|
| Default   | `--border`      | Transparent      | None                  | -                         |
| Focus     | `--indigo-400`  | Transparent      | 3px `--focus-ring`    | -                         |
| Error     | `--error-500`   | `--error-muted`  | 3px `--error-500/20%` | `aria-invalid="true"`     |
| Disabled  | `--border`      | `--hover`        | None                  | Opacity 0.6, not-allowed  |
| Read-only | `--border`      | `--hover`        | None                  | Cursor default            |

### Field Types

| Type       | Height | Padding | Additional                         |
|------------|--------|---------|------------------------------------|
| Text Input | 40px   | 12px    | Single-line, auto-capitalize off   |
| Textarea   | 80px+  | 12px    | Min-height 80px, resize vertical   |
| Select     | 40px   | 12px    | Chevron icon right-aligned         |
| Checkbox   | 20px   | -       | 20x20px, rounded-sm, indigo fill  |
| Radio      | 20px   | -       | 20x20px, circular, indigo fill    |
| Toggle     | 24px   | -       | 44x24px track, 20x20px thumb      |

### Validation Pattern

**Triggers:** On blur, on submit, after first error then on every change.

```html
<div class="form-field">
  <label for="email" class="block text-sm font-medium text-text-secondary mb-1">
    Email <span class="text-error-500">*</span>
  </label>
  <input type="email" id="email" name="email"
    class="w-full h-10 px-3 border border-border rounded-md"
    aria-required="true" aria-invalid="false">
  <p id="email-error" class="mt-1 text-sm text-error-500 hidden" role="alert"></p>
</div>
```

```javascript
const input = document.getElementById('email');
const errorEl = document.getElementById('email-error');

input.addEventListener('blur', () => {
  if (!input.validity.valid) {
    input.setAttribute('aria-invalid', 'true');
    input.classList.add('border-error-500', 'bg-error-muted');
    errorEl.textContent = input.validationMessage;
    errorEl.classList.remove('hidden');
  } else {
    input.setAttribute('aria-invalid', 'false');
    input.classList.remove('border-error-500', 'bg-error-muted');
    errorEl.classList.add('hidden');
  }
});
```

### Required Field Indicator

- Asterisk (*) after label in `--error-500`
- `aria-required="true"` on input
- Never use red label text (reserve for errors)

### Autocomplete Attributes

Always include for common fields:

```html
<input name="email" type="email" autocomplete="email">
<input name="password" type="password" autocomplete="current-password">
<input name="newPassword" type="password" autocomplete="new-password">
<input name="name" type="text" autocomplete="name">
<input name="tel" type="tel" autocomplete="tel">
<input name="address" type="text" autocomplete="street-address">
<input name="city" type="text" autocomplete="address-level2">
<input name="postalCode" type="text" autocomplete="postal-code">
```
