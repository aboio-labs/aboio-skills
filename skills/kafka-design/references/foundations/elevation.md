---
title: Elevation System and Shadows
impact: MEDIUM
impactDescription: Visual depth hierarchy for floating and layered elements
tags: elevation, shadows, depth, z-index, modals
---

## Elevation & Shadows

Subtle shadows create depth without distraction. Dark mode shadows are heavier.

### Shadow Tokens

| Token          | Light Mode                        | Dark Mode                         | Usage                |
|----------------|-----------------------------------|-----------------------------------|----------------------|
| `--shadow-sm`  | 0 1px 2px rgba(0,0,0,0.05)       | 0 1px 3px rgba(0,0,0,0.4)        | Subtle lift (hover)  |
| `--shadow-md`  | 0 2px 8px rgba(0,0,0,0.08)       | 0 2px 12px rgba(0,0,0,0.5)       | **Default cards**    |
| `--shadow-lg`  | 0 4px 16px rgba(0,0,0,0.12)      | 0 4px 20px rgba(0,0,0,0.6)       | Popovers, dropdowns  |
| `--shadow-xl`  | 0 8px 32px rgba(0,0,0,0.16)      | 0 8px 40px rgba(0,0,0,0.7)       | Modals, overlays     |
| `--shadow-focus`| 0 0 0 3px oklch(0.66 0.14 232 / 20%) | 0 0 0 3px oklch(0.66 0.14 232 / 30%) | Focus rings    |

### 5-Level Elevation Hierarchy

| Level | Name      | Shadow        | Usage                                |
|-------|-----------|---------------|--------------------------------------|
| 0     | Canvas    | None          | App background                       |
| 1     | Surface   | `--shadow-md` | Cards, panels                        |
| 2     | Raised    | `--shadow-lg` | Sticky headers, floating toolbars    |
| 3     | Overlay   | `--shadow-lg` | Dropdowns, popovers, tooltips        |
| 4     | Modal     | `--shadow-xl` | Modals, dialogs, full overlays       |

### Rules

- Shadows only appear on floating elements
- Never stack shadows (modal on modal gets same shadow)
- Hover states can increase shadow subtly (`--shadow-md` -> `--shadow-lg`)
- Dark mode uses higher opacity shadows because dark backgrounds absorb more light
