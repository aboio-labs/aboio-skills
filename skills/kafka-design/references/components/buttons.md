---
title: Button Variants, Sizes, and States
impact: CRITICAL
impactDescription: Primary interaction mechanism for all user actions
tags: buttons, variants, states, loading, accessibility
---

## Buttons

5 button variants with strict usage guidelines.

### Variants

| Variant        | Background       | Text             | Border           | Usage                       |
|----------------|------------------|------------------|------------------|-----------------------------|
| **Primary**    | `--indigo-400`   | White            | None             | Main actions (Save, Create) |
| **Secondary**  | Transparent      | `--text-primary` | 1px `--border`   | Secondary actions (Cancel)  |
| **Ghost**      | Transparent      | `--text-primary` | None             | Tertiary (icon buttons)     |
| **Destructive**| `--error-500`    | White            | None             | Delete, Remove, Discard     |
| **Link**       | Transparent      | `--indigo-400`   | None             | In-text links, navigation   |

### Sizes

| Size | Height | Padding H      | Font Size  | Icon Size | Usage                |
|------|--------|----------------|------------|-----------|----------------------|
| `sm` | 32px   | 12px (space-3) | 0.75rem    | 14px      | Dense tables, toolbars|
| `md` | 40px   | 16px (space-4) | 0.875rem   | 16px      | **Default**, most UI |
| `lg` | 48px   | 24px (space-6) | 0.875rem   | 18px      | Prominent CTAs       |

### States

| State      | Treatment                                         |
|------------|---------------------------------------------------|
| Default    | Base colors as specified                          |
| Hover      | Primary: `--indigo-500`, Secondary: `--hover` bg  |
| Active     | Primary: `--indigo-600`, Secondary: `--active` bg |
| Focus      | 3px ring `--focus-ring` with 2px offset           |
| Disabled   | Opacity 0.5, cursor not-allowed, no interactions  |
| Loading    | Show spinner, disable interaction, maintain width |

### HTML Examples

```html
<!-- Primary -->
<button type="button"
  class="h-10 px-4 bg-indigo-400 text-white rounded-md
         hover:bg-indigo-500 active:bg-indigo-600
         focus-visible:ring-3 focus-visible:ring-indigo-400/20">
  Save Product
</button>

<!-- Icon-only ghost (MUST have aria-label) -->
<button type="button" aria-label="Delete item"
  class="h-10 w-10 flex items-center justify-center
         text-text-secondary hover:bg-hover rounded-md
         focus-visible:ring-3">
  <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor">
    <path d="M3 6h18M19 6v14a2 2 0 01-2 2H7a2 2 0 01-2-2V6m3 0V4a2 2 0 012-2h4a2 2 0 012 2v2"/>
  </svg>
</button>
```

### Loading State

```html
<button type="button" class="h-10 px-4 bg-indigo-400 text-white rounded-md"
  disabled aria-busy="true">
  <span class="inline-flex items-center gap-2">
    <svg class="animate-spin w-4 h-4" viewBox="0 0 24 24" fill="none">
      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/>
      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/>
    </svg>
    Saving...
  </span>
</button>
```

```javascript
async function withLoadingState(button, action) {
  const originalText = button.textContent;
  button.disabled = true;
  button.setAttribute('aria-busy', 'true');
  button.innerHTML = `
    <span class="inline-flex items-center gap-2">
      <svg class="animate-spin w-4 h-4" viewBox="0 0 24 24" fill="none">
        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/>
        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/>
      </svg>
      Saving...
    </span>`;
  try {
    await action();
    button.textContent = originalText;
  } catch (error) {
    button.textContent = 'Error';
  } finally {
    button.disabled = false;
    button.removeAttribute('aria-busy');
  }
}
```

### Rules

**Do:**
- Use only one primary button per section
- Icon-only buttons MUST have `aria-label`
- Loading states MUST maintain button dimensions
- Destructive actions MUST have confirmation dialogs

**Don't:**
- Mix primary and destructive in the same action group
- Use all caps text (use sentence case)
- Create custom button variants outside these 5
- Use buttons for navigation (use Link variant)
