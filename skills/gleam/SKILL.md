---
name: gleam
description: Core Gleam language skill. Covers syntax fundamentals, type design, Parse Don't Validate, JSON decoding, pattern matching, tooling, and library design. Use when writing pure Gleam functions, validators, or shared domain types.
---

# Gleam Core Fundamentals

Consolidated skill for pure Gleam development. **Read the relevant reference before writing code.**

## Token Efficiency
**All agents and commands using this skill MUST follow `references/token-efficiency.md`.** Use `Read` and `Grep` tools with targeted reads instead of reading entire files. Start with `git diff` to scope work.

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

## Core Principles

1. **Errors as Values** — use Result types, never exceptions
2. **Explicit over Implicit** — no hidden state, no magic
3. **Type Safety** — let the compiler catch bugs
4. **Parse, Don't Validate** — opaque types for validated data
