---
title: Semantic Color Palettes (Success, Warning, Error, Info)
impact: CRITICAL
impactDescription: Status indicators for NF-e, fulfillment, alerts, and validation
tags: colors, semantic, success, warning, error, info, status, oklch
---

## Semantic Colors

Distinct hues ensure instant recognition across the interface.

### Success (Green, hue: 145)

| Token             | OKLCH Light                    | OKLCH Dark                     | Hex (Light / Dark)            | Usage              |
|-------------------|--------------------------------|--------------------------------|-------------------------------|--------------------|
| `--success-50`    | oklch(0.97 0.02 145)          | oklch(0.18 0.02 145)          | #f0fdf4 / #1a2e20            | Subtle bg          |
| `--success-100`   | oklch(0.94 0.04 145)          | oklch(0.22 0.04 145)          | #dcfce7 / #234029            | Hover              |
| `--success-200`   | oklch(0.88 0.08 145)          | oklch(0.28 0.08 145)          | #bbf7d0 / #2f543b            | Active             |
| `--success-400`   | oklch(0.66 0.14 145)          | oklch(0.66 0.14 145)          | #4ade80 / #4ade80            | Icons              |
| `--success-500`   | oklch(0.58 0.16 145)          | oklch(0.58 0.16 145)          | #22c55e / #22c55e            | **Primary**        |
| `--success-600`   | oklch(0.50 0.16 145)          | oklch(0.72 0.14 145)          | #16a34a / #6ee89f            | Emphasis           |
| `--success-muted` | oklch(0.58 0.16 145 / 12%)    | oklch(0.58 0.16 145 / 15%)    | rgba(34,197,94,0.12/0.15)    | Backgrounds        |

**When to use:** Authorized NF-e, successful fulfillment, positive financial metrics, approved settlements.

### Warning (Amber, hue: 70)

| Token             | OKLCH Light                    | OKLCH Dark                     | Hex (Light / Dark)            | Usage              |
|-------------------|--------------------------------|--------------------------------|-------------------------------|--------------------|
| `--warning-50`    | oklch(0.97 0.02 70)           | oklch(0.18 0.02 70)           | #fffbeb / #2e2415            | Subtle bg          |
| `--warning-100`   | oklch(0.94 0.05 70)           | oklch(0.22 0.05 70)           | #fef3c7 / #42331f            | Hover              |
| `--warning-200`   | oklch(0.88 0.10 70)           | oklch(0.28 0.10 70)           | #fde68a / #5a462a            | Active             |
| `--warning-400`   | oklch(0.75 0.16 70)           | oklch(0.75 0.16 70)           | #fbbf24 / #fbbf24            | Icons              |
| `--warning-500`   | oklch(0.68 0.18 70)           | oklch(0.68 0.18 70)           | #f59e0b / #f59e0b            | **Primary**        |
| `--warning-600`   | oklch(0.60 0.18 70)           | oklch(0.80 0.16 70)           | #d97706 / #fcd34d            | Emphasis           |
| `--warning-muted` | oklch(0.68 0.18 70 / 12%)     | oklch(0.68 0.18 70 / 15%)     | rgba(245,158,11,0.12/0.15)   | Backgrounds        |

**When to use:** Low stock alerts, pending NF-e, consignment aging >60 days, budget variance >10%.

### Error (Red, hue: 27)

| Token           | OKLCH Light                    | OKLCH Dark                     | Hex (Light / Dark)            | Usage              |
|-----------------|--------------------------------|--------------------------------|-------------------------------|--------------------|
| `--error-50`    | oklch(0.97 0.02 27)           | oklch(0.18 0.02 27)           | #fef2f2 / #2e1a1a            | Subtle bg          |
| `--error-100`   | oklch(0.94 0.04 27)           | oklch(0.22 0.04 27)           | #fee2e2 / #422424            | Hover              |
| `--error-200`   | oklch(0.88 0.09 27)           | oklch(0.28 0.09 27)           | #fecaca / #5a3232            | Active             |
| `--error-400`   | oklch(0.63 0.24 27)           | oklch(0.63 0.24 27)           | #f87171 / #f87171            | Icons              |
| `--error-500`   | oklch(0.55 0.26 27)           | oklch(0.55 0.26 27)           | #ef4444 / #ef4444            | **Primary**        |
| `--error-600`   | oklch(0.48 0.26 27)           | oklch(0.72 0.22 27)           | #dc2626 / #f99999            | Emphasis           |
| `--error-muted` | oklch(0.55 0.26 27 / 12%)     | oklch(0.55 0.26 27 / 15%)     | rgba(239,68,68,0.12/0.15)    | Backgrounds        |

**When to use:** Rejected NF-e, failed validation, negative financial variance, destructive actions.

### Info (Blue, hue: 240)

| Token          | OKLCH Light                    | OKLCH Dark                     | Hex (Light / Dark)            | Usage              |
|----------------|--------------------------------|--------------------------------|-------------------------------|--------------------|
| `--info-50`    | oklch(0.97 0.01 240)          | oklch(0.18 0.02 240)          | #eff6ff / #1a1f2e            | Subtle bg          |
| `--info-100`   | oklch(0.94 0.03 240)          | oklch(0.22 0.04 240)          | #dbeafe / #242a42            | Hover              |
| `--info-200`   | oklch(0.88 0.06 240)          | oklch(0.28 0.08 240)          | #bfdbfe / #32395a            | Active             |
| `--info-400`   | oklch(0.66 0.14 240)          | oklch(0.66 0.14 240)          | #60a5fa / #60a5fa            | Icons              |
| `--info-500`   | oklch(0.58 0.18 240)          | oklch(0.58 0.18 240)          | #3b82f6 / #3b82f6            | **Primary**        |
| `--info-600`   | oklch(0.50 0.19 240)          | oklch(0.72 0.16 240)          | #2563eb / #94c3ff            | Emphasis           |
| `--info-muted` | oklch(0.58 0.18 240 / 12%)    | oklch(0.58 0.18 240 / 15%)    | rgba(59,130,246,0.12/0.15)   | Backgrounds        |

**When to use:** Informational banners, helpful tips, neutral notifications, metadata previews.
