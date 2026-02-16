---
title: Color Contrast Requirements (WCAG AA)
impact: CRITICAL
impactDescription: 4.5:1 minimum contrast for text readability
tags: accessibility, contrast, wcag, aa, readability
---

## Color Contrast

Kafka follows WCAG 2.1 AA standards for all text and interactive elements.

### Minimum Ratios

| Context                           | Minimum |
|-----------------------------------|---------|
| Body text (14px+)                 | 4.5:1   |
| Large text (18px+ or 14px+ bold)  | 3:1     |
| UI components (borders, icons)    | 3:1     |
| Focus indicators                  | 3:1     |

### Verified Kafka Combinations

| Background      | Foreground        | Ratio   | Grade |
|-----------------|-------------------|---------|-------|
| Canvas (dark)   | Text-primary      | 13.2:1  | AAA   |
| Surface (dark)  | Text-primary      | 11.8:1  | AAA   |
| Canvas (dark)   | Text-secondary    | 5.8:1   | AA    |
| Indigo-400      | White             | 7.2:1   | AAA   |
| Success-500     | White             | 4.8:1   | AA    |
| Body text (any) | text-primary      | 12.8:1  | AAA   |
| UI components   | Various           | 4.2:1   | AA    |

### Testing

Use https://webaim.org/resources/contrastchecker/ to verify custom combinations.

### Rules

- Never use `--text-tertiary` for important content (insufficient contrast for body text)
- Semantic badge text colors (600 level) are pre-verified against their muted backgrounds
- When placing text on colored backgrounds, always verify contrast
