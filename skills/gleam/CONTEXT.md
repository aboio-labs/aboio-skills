# gleam — Core Gleam Skill
> Last updated: 2026-05-16

Covers language fundamentals, syntax, type design, and library construction.
For backend logic (Wisp, OTP) use the `gleam-backend` skill.
For frontend logic (Lustre) use the `lustre` skill.

Inherits: `skills/CONTEXT.md`.

## Layout
```
gleam/
├── SKILL.md                         # Main prompt with decision trees
└── references/
    ├── fundamentals/                # Language syntax, patterns, conventions
    │   ├── language-basics.md       # No if, records, imports, constants
    │   ├── language-features.md     # Label shorthand, let assert, pipelines
    │   ├── case-patterns.md         # Pattern matching
    │   ├── code-patterns.md         # use/result.try, helpers
    │   ├── conventions.md           # Official conventions (C1-C10) & anti-patterns (A1-A10)
    │   ├── error-handling.md        # AppError patterns
    │   ├── type-design.md           # Opaque types, parse-don't-validate
    │   ├── decoding.md              # JSON decoding (modern decode API)
    │   ├── decode-map-vs-then.md    # decode.map vs decode.then
    │   ├── stdlib.md                # Useful stdlib functions
    │   ├── stdlib-module-names.md   # Module name gotchas (regexp/regex)
    │   ├── tooling.md               # gleam check/format/fix, LSP actions
    │   ├── common-pitfalls.md       # Frequent mistakes
    │   ├── validation-valid.md      # Input validation (valid library)
    │   ├── parsing-nibble.md        # Parser combinators (nibble)
    │   ├── birdie-snapshot-testing.md
    │   ├── helper-first-refactoring.md
    │   ├── higher-order-sql-helpers.md
    │   └── string-character-filtering.md
    └── library/                     # Library design & FFI
        ├── library-design.md        # Effects as data, no IO in core
        └── ffi.md                   # Erlang + JavaScript FFI
```

## Reference routing — by task

| Task                          | References to read                                                               |
|-------------------------------|----------------------------------------------------------------------------------|
| New to Gleam syntax           | `fundamentals/language-basics.md` + `fundamentals/language-features.md`          |
| Input validation & Types      | `fundamentals/type-design.md` + `fundamentals/validation-valid.md`               |
| JSON Decoding / Encoding      | `fundamentals/decoding.md` + `fundamentals/decode-map-vs-then.md`                |
| Error handling & AppError     | `fundamentals/error-handling.md`                                                 |
| Designing a Library           | `library/library-design.md` + `library/ffi.md`                                   |

## Reference routing — by topic

| Topic                                    | Reference                                       |
|------------------------------------------|-------------------------------------------------|
| Official Conventions (C1-C10)            | `fundamentals/conventions.md`                   |
| Common mistakes / pitfalls               | `fundamentals/common-pitfalls.md`               |
| Pattern Matching                         | `fundamentals/case-patterns.md`                 |
| Code Patterns (use, result.try)          | `fundamentals/code-patterns.md`                 |
| Tooling (check, format, LSP)             | `fundamentals/tooling.md`                       |
| Snapshot testing (birdie)                | `fundamentals/birdie-snapshot-testing.md`       |

## Common Gleam gotchas

### Decoding
- **`decode.optional_field` crashes on `null`** — it only handles *missing* fields. If JSON sends `null`, use `Option(String)`
