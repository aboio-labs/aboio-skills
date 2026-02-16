---
title: Consignment Status Flow (Color-Coded Badges)
impact: HIGH
impactDescription: Visual lifecycle tracking for consignment operations
tags: publishing, consignment, status, badges, workflow
---

## Consignment Status Flow

Visualize consignment lifecycle with color-coded badges.

### Status Flow

| Step | Status      | Badge Variant | Color   |
|------|-------------|---------------|---------|
| 1    | **Draft**   | Info          | Blue    |
| 2    | **Pending** | Warning       | Amber   |
| 3    | **Active**  | Success       | Green   |
| 4    | **Settled** | Neutral       | Gray    |
| 5    | **Cancelled** | Error       | Red     |

### Badge HTML Examples

```html
<!-- Draft -->
<span class="inline-flex items-center h-6 px-2 bg-info-muted text-info-600
             text-xs font-medium rounded-sm border border-info-500/20">Draft</span>

<!-- Pending -->
<span class="inline-flex items-center h-6 px-2 bg-warning-muted text-warning-600
             text-xs font-medium rounded-sm border border-warning-500/20">Pending</span>

<!-- Active -->
<span class="inline-flex items-center h-6 px-2 bg-success-muted text-success-600
             text-xs font-medium rounded-sm border border-success-500/20">Active</span>

<!-- Settled -->
<span class="inline-flex items-center h-6 px-2 bg-hover text-text-secondary
             text-xs font-medium rounded-sm border border-border">Settled</span>

<!-- Cancelled -->
<span class="inline-flex items-center h-6 px-2 bg-error-muted text-error-600
             text-xs font-medium rounded-sm border border-error-500/20">Cancelled</span>
```

### Warning Triggers

- Consignment aging >60 days: show warning badge/icon
- Approaching settlement deadline: warning indicator
