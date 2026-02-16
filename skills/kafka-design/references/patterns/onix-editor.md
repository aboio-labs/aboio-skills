---
title: ONIX Metadata Editor (Tabbed UI)
impact: MEDIUM
impactDescription: Structured metadata editing interface for book records
tags: publishing, onix, metadata, tabs, editor
---

## ONIX Metadata Editor

Tabbed interface with clear information hierarchy for editing book metadata.

### Tab Structure

| Tab | Content                                        |
|-----|------------------------------------------------|
| 1   | **Basic Info:** Title, ISBN, Publisher, Pub Date|
| 2   | **Contributors:** Authors, Translators, Illustrators with roles|
| 3   | **Subjects:** BISAC, THEMA classifications     |
| 4   | **Descriptions:** Short, Long, Promotional     |
| 5   | **Supply:** Price, Availability, Supplier info  |

### Field Layout

- Single column, max-width 680px
- Label above input (not inline)
- Required fields marked with red asterisk
- Validation messages below field

### Design Rules

- Use the standard form field patterns from `components/forms.md`
- ISBN fields use `font-mono` with formatting (see `isbn-formatting.md`)
- Price fields use `tabular-nums` (see `currency-formatting.md`)
- Classification fields (BISAC, THEMA) use searchable dropdowns
- Contributor list uses drag-to-reorder for sequencing
