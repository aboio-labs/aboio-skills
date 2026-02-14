---
name: gleam
description: Comprehensive Gleam skill covering language fundamentals, backend (OTP,
  Squirrel, HTTP, auth), frontend (Lustre 5.x, rsvp, modem, plinth), and library design.
  Use for any Gleam development task.
---

# Gleam Best Practices

Consolidated skill for Gleam development. Use decision trees below to find the right
reference, then load detailed docs.

## Token Efficiency

**All agents and commands using this skill MUST follow `references/token-efficiency.md`.** Use Grep and targeted reads instead of reading entire files. Start with `git diff` to scope work.

## Core Principles

1. **Errors as Values** — use Result types, never exceptions
2. **Explicit over Implicit** — no hidden state, no magic
3. **Type Safety** — let the compiler catch bugs
4. **Layered Architecture** — separate concerns
5. **Parse, Don't Validate** — opaque types for validated data

## Quick Decision Trees

### "I need to write Gleam code"

    Writing Gleam?
    ├─ New to syntax (no if, records, imports)  → fundamentals/language-basics.md
    ├─ Case expressions, pattern matching       → fundamentals/case-patterns.md
    ├─ Label shorthand, let assert, pipelines   → fundamentals/language-features.md
    ├─ Useful stdlib functions                  → fundamentals/stdlib.md
    ├─ Module name mistakes (regexp/regex)      → fundamentals/stdlib-module-names.md
    ├─ String character filtering (no \0)       → fundamentals/string-character-filtering.md
    ├─ JSON decoding/encoding                   → fundamentals/decoding.md
    ├─ decode.map vs decode.then                → fundamentals/decode-map-vs-then.md
    ├─ Opaque types, module design              → fundamentals/type-design.md
    ├─ Error discipline, AppError patterns      → fundamentals/error-handling.md
    ├─ use/result.try, helpers                  → fundamentals/code-patterns.md
    ├─ Helper-first refactoring                 → fundamentals/helper-first-refactoring.md
    ├─ Higher-order SQL helpers                 → fundamentals/higher-order-sql-helpers.md
    ├─ Tooling (gleam check, format, LSP)       → fundamentals/tooling.md
    ├─ Input validation (valid)                 → fundamentals/validation-valid.md
    ├─ Parser combinators (nibble)              → fundamentals/parsing-nibble.md
    └─ Common mistakes                          → fundamentals/common-pitfalls.md

### "I need to build a backend"

    Backend (Erlang target)?
    ├─ Web framework (wisp)                     → backend/wisp-framework.md
    ├─ HTTP server (mist)                       → backend/mist-server.md
    ├─ HTTP logging (single middleware)         → backend/http-logging-middleware.md
    ├─ DB error handling (3-tier)               → backend/three-tier-error-handling.md
    ├─ Decoder defaults anti-pattern            → backend/decoder-defaults-anti-pattern.md
    ├─ OTP actors (basics, state, messages)     → backend/otp.md
    ├─ OTP supervision (trees, strategies)      → backend/otp-supervision.md
    ├─ OTP advanced (selectors, timers, ETS)    → backend/otp-advanced.md
    ├─ Production logging (wisp)                → Use /observability-master command
    ├─ HTTP client runner                       → backend/http-runner.md
    ├─ Midas algebraic effects                  → backend/midas-effect-task.md
    ├─ SQL code generation (Squirrel)           → backend/squirrel-guide.md
    ├─ Database migrations (Cigogne)           → backend/cigogne.md
    ├─ JWT authentication (ywt)                 → backend/jwt-ywt.md
    ├─ S3 / object storage (bucket)             → backend/bucket-s3.md
    └─ Password hashing, timestamps             → backend/auth.md

### "I need to build a frontend"

    Frontend (Lustre / JS target)?
    ├─ MVU architecture, state, messages        → frontend/lustre-core.md
    ├─ Effects, paint-cycle, context            → frontend/lustre-effects.md
    ├─ SPA routing (modem)                      → frontend/lustre-routing.md
    ├─ HTTP requests (rsvp)                     → frontend/lustre-http.md
    ├─ Web components, slots, CSS parts         → frontend/lustre-components.md
    ├─ Events, debounce, throttle               → frontend/lustre-events.md
    ├─ Composable UI, prop pattern, views       → frontend/lustre-ui-patterns.md
    ├─ Hydration, server components, FFI        → frontend/lustre-advanced.md
    ├─ Browser APIs (plinth)                    → frontend/lustre-browser-apis.md
    └─ Common Lustre gotchas                    → frontend/lustre-gotchas.md

### "I need to design a library"

    Library design?
    ├─ Effects as data, no IO in core           → library/library-design.md
    └─ FFI (Erlang + JavaScript)                → library/ffi.md

### "I need to review OTP code"

    OTP review (after implementing actors/supervision)?
    └─ Audit actors, supervision, concurrency   → Use /otp-review command

**When to use /otp-review:**
- After creating new actors
- After modifying supervision trees
- After changing actor state or message types
- When experiencing timeouts, deadlocks, or memory leaks
- Before committing OTP-related code

Read at most 2 reference files per turn to stay token-efficient.
