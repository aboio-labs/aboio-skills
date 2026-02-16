---
title: Border Radius Scale
impact: MEDIUM
impactDescription: Consistent rounding for modern, polished feel
tags: border-radius, rounding, cards, buttons, badges
---

## Border Radius

Subtle, consistent rounding. Modern and polished without excessive softness.

### Scale Table

| Token           | Value   | Usage                                  |
|-----------------|---------|----------------------------------------|
| `--radius-sm`   | 4px     | Badges, tags, small pills              |
| `--radius-md`   | 6px     | **Default**: inputs, buttons, cards    |
| `--radius-lg`   | 8px     | Large cards, modals, popovers          |
| `--radius-xl`   | 12px    | Hero cards, prominent panels           |
| `--radius-2xl`  | 16px    | Large modals, full-screen panels       |
| `--radius-full` | 9999px  | Avatars, status indicators (circular)  |

### CSS Custom Properties

```css
:root {
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
  --radius-xl: 12px;
  --radius-2xl: 16px;
  --radius-full: 9999px;
}
```

### Guidelines

- Use `--radius-md` (6px) by default for consistency
- Reserve `--radius-lg` and larger for prominent UI elements
- Never mix multiple radius values on adjacent elements
- Use `--radius-full` only for circular elements (avatars, status dots)
