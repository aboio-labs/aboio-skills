---
title: Color Usage Guidelines and Contrast Requirements
impact: CRITICAL
impactDescription: WCAG 2.1 AA compliance and visual hierarchy rules
tags: colors, contrast, wcag, accessibility, hierarchy
---

## Color Usage Guidelines

### Hierarchy & Emphasis

1. **Primary actions:** Always `--indigo-400` for buttons, links, CTAs
2. **Organizational identity:** `--violet-400` for org badges, workspace indicators
3. **Status indicators:** Semantic colors (success/warning/error/info) for badges and alerts
4. **Text hierarchy:** Primary -> Secondary -> Tertiary for reading comfort
5. **Surface elevation:** Canvas -> Surface -> Surface-raised creates depth

### Dual-Accent System

Kafka uses two accent hues for distinct purposes:
- **Indigo (232):** System actions, navigation, interactive elements
- **Violet (270):** Organization identity, categories, secondary branding

Never mix these roles. Indigo = action, Violet = identity.

### WCAG 2.1 AA Contrast Requirements

| Context                    | Minimum Ratio |
|----------------------------|---------------|
| Body text (14px+)          | 4.5:1         |
| Large text (18px+ or 14px bold) | 3:1     |
| UI components (borders, icons) | 3:1      |
| Focus indicators           | 3:1           |

### Verified Combinations

| Background      | Foreground        | Ratio   | Pass   |
|-----------------|-------------------|---------|--------|
| Canvas (dark)   | Text-primary      | 13.2:1  | AAA    |
| Surface (dark)  | Text-primary      | 11.8:1  | AAA    |
| Canvas (dark)   | Text-secondary    | 5.8:1   | AA     |
| Indigo-400      | White             | 7.2:1   | AAA    |
| Success-500     | White             | 4.8:1   | AA     |

### Color Misuse (Don't)

- Don't use semantic colors for decoration (green borders on non-success elements)
- Don't mix indigo and violet in the same action group
- Don't use color as the only way to convey information (add icons/text)
- Don't use light-mode-specific hex values in dark mode
- Don't create custom accent colors outside indigo/violet

### Tools

- Contrast Checker: https://webaim.org/resources/contrastchecker/
- OKLCH Color Picker: https://oklch.com
