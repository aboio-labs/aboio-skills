---
title: Card Variants, Padding, and Elevation
impact: HIGH
impactDescription: Primary content container providing visual grouping and hierarchy
tags: cards, variants, padding, elevation, interactive
---

## Cards

Primary content container in Kafka. Provides visual grouping and hierarchy.

### Anatomy

```
+----------------------------------+
|  [Title]                         |  <- Header (optional)
|  -------------------------------- |
|                                  |  <- Content area (padding: space-5)
|  Content goes here               |
|                                  |
|  -------------------------------- |
|  [Action]  [Action]              |  <- Footer (optional)
+----------------------------------+
```

### Variants

| Variant         | Background          | Border           | Shadow        | Usage                  |
|-----------------|---------------------|------------------|---------------|------------------------|
| **Default**     | `--surface`         | 1px `--border`   | `--shadow-md` | Standard content cards |
| **Raised**      | `--surface-raised`  | None             | `--shadow-lg` | Prominent cards, modals|
| **Flat**        | `--surface`         | 1px `--border`   | None          | Embedded cards         |
| **Interactive** | `--surface`         | 1px `--border`   | `--shadow-md` | Clickable cards        |

### Interactive Card States

```css
.card-interactive {
  background: var(--surface);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-md);
  transition: box-shadow 150ms ease, border-color 150ms ease;
}

.card-interactive:hover {
  border-color: var(--indigo-400);
  box-shadow: var(--shadow-lg);
}

.card-interactive:active {
  box-shadow: var(--shadow-sm);
}
```

### Padding Scale

| Context            | Padding         | Usage                        |
|--------------------|-----------------|------------------------------|
| Dense list items   | 12px (space-3)  | Product list, order list     |
| Standard cards     | 20px (space-5)  | **Default**, dashboard cards |
| Large cards        | 24px (space-6)  | Detail editors, settings     |
| Modal content      | 32px (space-8)  | Modal body content           |
