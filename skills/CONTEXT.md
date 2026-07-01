# skills — Skill packages
> Last updated: 2026-04-25

Each skill is a directory containing a `SKILL.md` (the prompt loaded
into context) and optionally a `references/` subdirectory with detailed
docs that get loaded on demand.

Inherits: top-level `CONTEXT.md`.

## Anatomy of a skill
```
skills/<name>/
├── SKILL.md             # Frontmatter (name, description) + main prompt
└── references/          # Detailed reference docs (loaded on demand)
    ├── topic-a.md
    └── topic-b.md
```

`SKILL.md` is kept small — it provides decision trees and routing to
reference files. Reference files contain the detailed patterns, examples,
and rules. This two-tier design keeps token usage low.

## Skill inventory

### Language & framework
| Skill               | Purpose                                           | References |
|----------------------|---------------------------------------------------|------------|
| `gleam`              | Core Gleam: syntax, types, decoding, library design | 21 files across 2 categories |
| `api-gleam`          | REST/JSON API patterns for Gleam + Wisp + Mist    | 9 files    |
| `pg-gleam`           | Postgres performance and best practices            | 42 files across 8 categories |

### Development workflows
| Skill               | Purpose                                           | References |
|----------------------|---------------------------------------------------|------------|
| `tdd`                | Test-Driven Development: RED → GREEN → REFACTOR   | 3 files    |
| `frontend-testing`   | Lustre frontend testing: model, update, behavior   | 1 file     |
| `code-simplifier`    | Refine recently modified Gleam code for clarity    | standalone |
| `observability-master` | Find unlogged error branches, add wisp.log_* calls | standalone |
| `test-reviewer`      | Review test quality                                | standalone |

## Sub-workspaces
| Working on...                    | Read                                  |
|----------------------------------|---------------------------------------|
| Main Gleam skill                 | `gleam/CONTEXT.md`                    |
| Postgres skill                   | `pg-gleam/CONTEXT.md`                 |
| API design skill                 | `api-gleam/CONTEXT.md`                |
| Creating a new skill             | `CONTRIBUTING.md` (top-level)         |

## Known coverage gaps

General Gleam patterns not yet fully covered by dedicated skill references:

- **Batch SQL operations** — Parrot/Squirrel can't generate UNNEST
  queries; the hand-written pattern needs documenting
- **Rollback-based test isolation with pog** — no nested
  `pog.transaction` inside rollback contexts (inner COMMIT kills
  the outer rollback)

## Adding a new skill
1. Create `skills/<name>/SKILL.md` with frontmatter
2. Add reference files in `skills/<name>/references/`
3. Add entry to `registry.json`
4. If it needs a slash command, add `commands/<name>.md`
