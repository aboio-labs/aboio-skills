---
title: Performance Guidelines (Code Splitting, Lazy Loading)
impact: MEDIUM
impactDescription: Optimal loading performance for dense publishing interfaces
tags: performance, code-splitting, lazy-load, images, critical-css
---

## Performance

### Critical Rendering Path

- Inline critical CSS (<14KB)
- Defer non-critical CSS
- Use system fonts as fallback (zero load time)
- Lazy load images with `loading="lazy"`

### Code Splitting Targets

| Route             | Target Size |
|-------------------|-------------|
| Dashboard         | ~45KB       |
| Product catalog   | ~62KB       |
| Orders            | ~58KB       |
| Financial reports | ~72KB       |

### Image Optimization

- Use WebP format with JPEG fallback
- Serve responsive images via `srcset`
- Lazy load images below fold
- Thumbnail sizes: 40x40, 80x80, 200x200

### Font Performance

- Preload only critical weights (400, 500, 600)
- Use `font-display: swap` for all @font-face
- Serve WOFF2 format only (best compression)
