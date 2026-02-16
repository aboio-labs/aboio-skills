---
name: kafka-design-system
description:
  Kafka ERP design system - colors (indigo/violet), typography (Iosevka),
  components, accessibility (WCAG 2.1 AA), publishing patterns. Dark-mode-first,
  information-dense interface. Use for UI/UX implementation.
---

# Kafka ERP Design System

Token-efficient design system for Kafka ERP. Use decision trees to find references,
then load specific files as needed.

## Token Efficiency

**Use Grep and targeted reads.** Load only the specific reference file needed. Don't
load multiple files unless absolutely necessary.

## Core Principles

1. **Dark Mode First** — design for extended professional use
2. **Information Density** — actionable visibility over decoration
3. **Accessibility Always** — WCAG 2.1 AA is mandatory, not optional
4. **Publishing Native** — understand books, ISBNs, consignments
5. **Pure HTML/JS** — no framework-specific examples (Gleam/Lustre compatible)

## Quick Decision Trees

### "I need color values"

    Colors?
    ├─ Primary actions (buttons, links)         → foundations/colors-indigo.md
    ├─ Org identity (badges, tags)              → foundations/colors-violet.md
    ├─ Status indicators (success/error/warn)   → foundations/colors-semantic.md
    ├─ Text, borders, backgrounds               → foundations/colors-neutral.md
    ├─ Contrast ratios, WCAG compliance         → foundations/colors-usage.md
    └─ Dark mode color tokens                   → foundations/colors-dark-mode.md

### "I need typography specs"

    Typography?
    ├─ Font loading (Iosevka Web)               → foundations/typography-fonts.md
    ├─ Type scale (display/title/body/caption)  → foundations/typography-scale.md
    ├─ Numeric data (tabular nums)              → foundations/typography-numbers.md
    ├─ Hierarchy rules                          → foundations/typography-hierarchy.md
    └─ Line length, readability                 → foundations/typography-rules.md

### "I need spacing or layout"

    Spacing/Layout?
    ├─ Spacing scale (4px base unit)            → foundations/spacing.md
    ├─ Border radius values                     → foundations/radius.md
    ├─ Shadows and elevation                    → foundations/elevation.md
    ├─ Three-column manager pattern             → patterns/three-column-layout.md
    ├─ Content width constraints                → patterns/content-width.md
    └─ Vertical rhythm                          → patterns/vertical-rhythm.md

### "I need to build a component"

    Component?
    ├─ Buttons (5 variants, states, sizes)      → components/buttons.md
    ├─ Form fields (inputs, validation)         → components/forms.md
    ├─ Cards (variants, padding, elevation)     → components/cards.md
    ├─ Badges (status, count, org identity)     → components/badges.md
    ├─ Data tables (sortable, responsive)       → components/tables.md
    ├─ Modals (focus trap, keyboard nav)        → components/modals.md
    ├─ Alerts & toasts (auto-dismiss)           → components/alerts.md
    ├─ Icons (sizes, colors, aria-label)        → components/icons.md
    └─ Common component mistakes                → components/common-mistakes.md

### "I need accessibility guidance"

    Accessibility?
    ├─ Keyboard navigation (Tab, focus trap)    → patterns/accessibility-keyboard.md
    ├─ Screen readers (ARIA, semantic HTML)     → patterns/accessibility-screen-readers.md
    ├─ Focus states (never remove outline)      → patterns/accessibility-focus.md
    ├─ Color contrast (4.5:1 minimum)           → patterns/accessibility-contrast.md
    ├─ Touch targets (44x44px minimum)          → patterns/accessibility-touch.md
    └─ Motion & animation (prefers-reduced)     → patterns/accessibility-motion.md

### "I need publishing-specific patterns"

    Publishing patterns?
    ├─ ISBN formatting (978-X-XX-XXXXXX-X)      → patterns/isbn-formatting.md
    ├─ Currency display (BRL/USD/EUR)           → patterns/currency-formatting.md
    ├─ Book covers (thumbnails, aspect ratio)   → patterns/book-covers.md
    ├─ Consignment status (color-coded flow)    → patterns/consignment-status.md
    └─ ONIX metadata editor (tabbed UI)         → patterns/onix-editor.md

### "I need implementation guidance"

    Implementation?
    ├─ CSS architecture (custom properties)     → patterns/css-architecture.md
    ├─ Dark mode implementation                 → patterns/dark-mode.md
    ├─ Component checklist (before shipping)    → patterns/component-checklist.md
    ├─ Performance (code splitting, lazy load)  → patterns/performance.md
    └─ Responsive strategy (breakpoints)        → patterns/responsive.md

## Reference Index

### Foundations

| Topic                    | Reference                                        |
| ------------------------ | ------------------------------------------------ |
| Indigo primary colors    | `references/foundations/colors-indigo.md`        |
| Violet secondary colors  | `references/foundations/colors-violet.md`        |
| Semantic colors (status) | `references/foundations/colors-semantic.md`      |
| Neutral colors (text/bg) | `references/foundations/colors-neutral.md`       |
| Color usage & contrast   | `references/foundations/colors-usage.md`         |
| Dark mode colors         | `references/foundations/colors-dark-mode.md`     |
| Font loading (Iosevka)   | `references/foundations/typography-fonts.md`     |
| Type scale               | `references/foundations/typography-scale.md`     |
| Numeric typography       | `references/foundations/typography-numbers.md`   |
| Typography hierarchy     | `references/foundations/typography-hierarchy.md` |
| Typography rules         | `references/foundations/typography-rules.md`     |
| Spacing scale            | `references/foundations/spacing.md`              |
| Border radius            | `references/foundations/radius.md`               |
| Elevation & shadows      | `references/foundations/elevation.md`            |

### Components

| Topic              | Reference                                  |
| ------------------ | ------------------------------------------ |
| Buttons            | `references/components/buttons.md`         |
| Forms & validation | `references/components/forms.md`           |
| Cards              | `references/components/cards.md`           |
| Badges             | `references/components/badges.md`          |
| Data tables        | `references/components/tables.md`          |
| Modals & dialogs   | `references/components/modals.md`          |
| Alerts & toasts    | `references/components/alerts.md`          |
| Icons              | `references/components/icons.md`           |
| Common mistakes    | `references/components/common-mistakes.md` |

### Patterns

| Topic                  | Reference                                             |
| ---------------------- | ----------------------------------------------------- |
| Three-column layout    | `references/patterns/three-column-layout.md`          |
| Content width          | `references/patterns/content-width.md`                |
| Vertical rhythm        | `references/patterns/vertical-rhythm.md`              |
| Keyboard accessibility | `references/patterns/accessibility-keyboard.md`       |
| Screen reader support  | `references/patterns/accessibility-screen-readers.md` |
| Focus states           | `references/patterns/accessibility-focus.md`          |
| Color contrast         | `references/patterns/accessibility-contrast.md`       |
| Touch targets          | `references/patterns/accessibility-touch.md`          |
| Motion & animation     | `references/patterns/accessibility-motion.md`         |
| ISBN formatting        | `references/patterns/isbn-formatting.md`              |
| Currency formatting    | `references/patterns/currency-formatting.md`          |
| Book covers            | `references/patterns/book-covers.md`                  |
| Consignment status     | `references/patterns/consignment-status.md`           |
| ONIX metadata editor   | `references/patterns/onix-editor.md`                  |
| CSS architecture       | `references/patterns/css-architecture.md`             |
| Dark mode impl         | `references/patterns/dark-mode.md`                    |
| Component checklist    | `references/patterns/component-checklist.md`          |
| Performance            | `references/patterns/performance.md`                  |
| Responsive strategy    | `references/patterns/responsive.md`                   |

## Common Queries

**"What color for primary button?"** → `colors-indigo.md` (--indigo-400)
**"How to format ISBN?"** → `isbn-formatting.md`
**"Button loading state?"** → `buttons.md` (see States section)
**"Form validation pattern?"** → `forms.md` (Constraint Validation API)
**"Minimum touch target?"** → `accessibility-touch.md` (44x44px)
**"Focus ring color?"** → `colors-indigo.md` + `accessibility-focus.md`
**"Modal keyboard handling?"** → `modals.md` (complete focus trap example)
**"Dark mode CSS setup?"** → `dark-mode.md` (data-theme attribute)
