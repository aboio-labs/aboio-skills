---
title: Book Cover Thumbnails (Sizes, Aspect Ratio)
impact: MEDIUM
impactDescription: Consistent book cover display across list, card, and detail views
tags: publishing, book-covers, thumbnails, aspect-ratio, images
---

## Book Cover Thumbnails

### Sizes

| Context     | Size    | Aspect Ratio | Border            |
|-------------|---------|--------------|-------------------|
| List view   | 40x60   | 2:3          | 1px `--border`    |
| Card view   | 120x180 | 2:3          | 1px `--border`    |
| Detail view | 200x300 | 2:3          | 1px `--border`    |
| Placeholder | Any     | 2:3          | Dashed, gray icon |

### CSS

```css
.book-cover {
  aspect-ratio: 2/3;
  object-fit: cover;
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
}

.book-cover-placeholder {
  aspect-ratio: 2/3;
  border: 1px dashed var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-tertiary);
  background: var(--surface);
}
```

### Image Optimization

- Use WebP format with JPEG fallback
- Serve responsive images via `srcset`
- Lazy load images below fold: `loading="lazy"`
- Generate thumbnails: 40x60, 80x120, 200x300
