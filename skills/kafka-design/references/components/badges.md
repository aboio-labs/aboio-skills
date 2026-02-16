---
title: Badge Variants, Sizes, and Count Badges
impact: HIGH
impactDescription: Status communication at a glance for orders, NF-e, consignments
tags: badges, status, count, tags, semantic-colors
---

## Badges

Communicate status, categories, and counts. Must be scannable at a glance.

### Variants

| Variant     | Background           | Text               | Border                  | Usage                |
|-------------|----------------------|--------------------|-------------------------|----------------------|
| **Primary** | `--indigo-400/10%`   | `--indigo-400`     | 1px `--indigo-400/20%`  | Neutral status       |
| **Success** | `--success-muted`    | `--success-600`    | 1px `--success-500/20%` | Active, Authorized   |
| **Warning** | `--warning-muted`    | `--warning-600`    | 1px `--warning-500/20%` | Pending, Low Stock   |
| **Error**   | `--error-muted`      | `--error-600`      | 1px `--error-500/20%`   | Rejected, Cancelled  |
| **Info**    | `--info-muted`       | `--info-600`       | 1px `--info-500/20%`    | Draft, Scheduled     |
| **Neutral** | `--hover`            | `--text-secondary` | 1px `--border`          | Tags, categories     |

### Sizes

| Size | Height | Padding H | Font Size   | Font Weight | Usage                |
|------|--------|-----------|-------------|-------------|----------------------|
| `sm` | 20px   | 6px       | 0.6875rem   | 500         | Table cells, dense UI|
| `md` | 24px   | 8px       | 0.75rem     | 500         | **Default**          |
| `lg` | 28px   | 10px      | 0.8125rem   | 500         | Prominent status     |

### Badge with Icon

```html
<span class="inline-flex items-center gap-1 h-6 px-2 bg-success-muted
             text-success-600 text-xs font-medium rounded-sm
             border border-success-500/20">
  <svg class="w-3 h-3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <path d="M20 6L9 17l-5-5"/>
  </svg>
  Authorized
</span>
```

### Count Badges (Circular)

For notification counts:

```html
<button type="button" class="relative">
  <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor">
    <path d="M18 8A6 6 0 006 8c0 7-3 9-3 9h18s-3-2-3-9M13.73 21a2 2 0 01-3.46 0"/>
  </svg>
  <span class="absolute -top-1 -right-1 flex items-center justify-center
               w-5 h-5 bg-error-500 text-white text-xs font-medium rounded-full">
    3
  </span>
</button>
```
