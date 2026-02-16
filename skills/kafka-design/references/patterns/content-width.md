---
title: Content Width Constraints
impact: MEDIUM
impactDescription: Optimal reading and scanning widths by context
tags: layout, max-width, content, forms, modals
---

## Content Width Constraints

| Context          | Max Width | Reasoning                                    |
|------------------|-----------|----------------------------------------------|
| Page content     | 1200px    | Optimal for 1440px+ displays                 |
| Reading content  | 680px     | Comfortable line length (65ch at 0.875rem)   |
| Forms            | 560px     | Easy scanning, prevents horizontal eye strain|
| Modals           | 600px     | Focused task completion (default size)        |
| Detail editors   | None      | Use available space for metadata tables       |

### CSS Example

```css
.page-content { max-width: 1200px; margin-inline: auto; }
.reading-content { max-width: 680px; }
.form-content { max-width: 560px; }
```
