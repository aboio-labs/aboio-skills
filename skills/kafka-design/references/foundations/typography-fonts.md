---
title: Font Loading and Iosevka Configuration
impact: HIGH
impactDescription: Primary typeface setup for geometric, information-dense rendering
tags: typography, fonts, iosevka, font-loading, woff2
---

## Iosevka Font System

Kafka uses **Iosevka** as its primary typeface. Geometric, information-dense, designed for technical interfaces.

### Why Iosevka

- Geometric precision and technical aesthetic
- Information-dense: narrow character width allows more content per line
- Excellent legibility at small sizes (12px+)
- Open source with customizable variants
- Consistent metrics across weights

### Font Stack

```css
/* Primary (Iosevka) */
--font-sans: "Iosevka Web", "Iosevka", ui-monospace,
             "SF Mono", Menlo, Monaco, Consolas, monospace;

/* Fallback Monospace */
--font-mono: "Iosevka Web", "Iosevka", ui-monospace,
             SFMono-Regular, "SF Mono", Menlo, Monaco, Consolas, monospace;
```

### Font Loading

Preload critical weights for optimal performance:

```html
<link rel="preload" href="/fonts/iosevka-regular.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="/fonts/iosevka-medium.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="/fonts/iosevka-semibold.woff2" as="font" type="font/woff2" crossorigin>
```

### @font-face Declarations

```css
@font-face {
  font-family: 'Iosevka Web';
  src: url('/fonts/iosevka-regular.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Iosevka Web';
  src: url('/fonts/iosevka-medium.woff2') format('woff2');
  font-weight: 500;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Iosevka Web';
  src: url('/fonts/iosevka-semibold.woff2') format('woff2');
  font-weight: 600;
  font-style: normal;
  font-display: swap;
}
```

### Allowed Weights

Only three weights for consistency and performance:

| Weight | Name     | Usage                                                |
|--------|----------|------------------------------------------------------|
| 400    | Regular  | All body text, descriptions, secondary information   |
| 500    | Medium   | Labels, button text, table headers, form field labels|
| 600    | Semibold | Headings (display, title, subtitle), emphasized data |

**Never use:** 300 (Light), 700 (Bold), 800+ (Extra Bold).

### Source

Download from https://github.com/be5invis/Iosevka or use a customized build with SS08 style set for optimal UI rendering.
