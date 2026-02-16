---
title: Vertical Rhythm Guidelines
impact: MEDIUM
impactDescription: Consistent vertical spacing using 4px base unit multiples
tags: spacing, vertical-rhythm, sections, headings, paragraphs
---

## Vertical Rhythm

Maintain consistent vertical spacing using multiples of the 4px base unit.

### Spacing Rules

| Transition           | Spacing         | Token    |
|----------------------|-----------------|----------|
| Heading to body      | 12px            | space-3  |
| Body to heading      | 24px            | space-6  |
| Paragraph spacing    | 16px            | space-4  |
| Section spacing      | 32px            | space-8  |
| Major section dividers| 48px           | space-12 |

### Example

```html
<section class="mb-12"> <!-- space-12 between major sections -->
  <h2 class="text-title mb-3">Section Title</h2> <!-- space-3 to body -->
  <p class="text-body mb-4">First paragraph.</p> <!-- space-4 between paragraphs -->
  <p class="text-body mb-6">Second paragraph.</p> <!-- space-6 before next heading -->
  <h3 class="text-subtitle mb-3">Subsection</h3>
  <p class="text-body">Content.</p>
</section>
```
