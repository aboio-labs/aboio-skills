---
description: Review Gleam code against best practices
argument-hint: [file-or-directory] [--thorough]
allowed-tools: [Read, Glob, Grep]
---

# Gleam Code Review

Review Gleam code against the full best practices reference — fundamentals, official conventions, error handling, type design, and domain-specific patterns.

## Instructions

### Step 0: Determine review mode

Check `$ARGUMENTS` for `--thorough` flag:

- **Default (light):** Categorized findings with file:line, severity tag, suggested fix, summary count.
- **Thorough (`--thorough`):** Full audit — numbered violations (V-001), before/after code for every finding, summary table, health score (Critical/Degraded/Healthy), clean files acknowledgment.

### Step 1: Load core references

Read these foundational reference files:

- `~/.claude/skills/gleam/references/fundamentals/common-pitfalls.md`
- `~/.claude/skills/gleam/references/fundamentals/error-handling.md`
- `~/.claude/skills/gleam/references/fundamentals/type-design.md`
- `~/.claude/skills/gleam/references/fundamentals/code-patterns.md`
- `~/.claude/skills/gleam/references/fundamentals/conventions.md`

### Step 2: Detect target domain

Examine the target files' imports and the project's `gleam.toml` to determine whether this is backend (Erlang target) or frontend (JavaScript target) code:

- **Backend indicators:** imports from `gleam/otp`, `gleam/erlang`, `wisp`, `mist`, `sqlight`, `pog`, `ywt`, `bucket`, `logging`
- **Frontend indicators:** imports from `lustre`, `lustre/element`, `lustre/effect`, `modem`, `rsvp`, `plinth`, `nibble`
- **Cross-cutting:** imports from `valid` (used in both targets)

### Step 3: Load domain-specific references

Based on the detected domain, load the base references:

**If backend:**
- `~/.claude/skills/gleam/references/backend/otp.md`
- `~/.claude/skills/gleam/references/backend/http-runner.md`
- `~/.claude/skills/gleam/references/fundamentals/decoding.md`

**If frontend:**
- `~/.claude/skills/gleam/references/frontend/lustre-gotchas.md`
- `~/.claude/skills/gleam/references/frontend/lustre-core.md`
- `~/.claude/skills/gleam/references/frontend/lustre-effects.md`

**If both or unclear**, load references from both domains.

Then check imports for specific libraries and load their references too:

| Import detected | Load reference |
|-----------------|----------------|
| `wisp` | `references/backend/wisp-framework.md` |
| `mist` | `references/backend/mist-server.md` |
| `valid` | `references/fundamentals/validation-valid.md` |
| `bucket` | `references/backend/bucket-s3.md` |
| `nibble` | `references/fundamentals/parsing-nibble.md` |
| `ywt` | `references/backend/jwt-ywt.md` |
| `logging` | `references/backend/logging.md` |

Only load what the code actually uses — don't load all of them.

### Step 4: Scan the target code efficiently

**Follow `references/token-efficiency.md` rules.** Do NOT read entire files.

If `$ARGUMENTS` specifies a file or directory, scan that. Otherwise, scan recently modified `.gleam` files.

1. Run `git diff --name-only` to identify changed files (prefer these over scanning all of `src/`)
2. Use Grep to find specific patterns across target files (see detection patterns below)
3. Read only specific line ranges around matches (offset+limit)
4. Only read full files when you need to understand module structure for safe edits

### Step 5: Run detection patterns

Scan for violations in each category using targeted Grep patterns. Run these checks in parallel where possible.

**§1 Type Design:**
- `pub type ` without `opaque` for validation types
- `Option(Option(` — nested options
- `Bool` fields in record types (especially 2+ bools per record)
- `is_active`, `is_loading`, `has_permission` — bool fields that should be custom types

**§2 Error Handling:**
- `Error(_) ->` — silently discarded errors (OK when error type is `Nil`)
- `fn(_err)` or `fn(_error)` — discarded errors in lambdas
- `let assert` in non-test, non-OTP code — should use `result.try`
- `panic` — should be extremely rare outside OTP

**§3 Code Patterns:**
- Nested `case` expressions (2+ levels deep)
- `case.*True` / `case.*False` — boolean case chains that could use `bool.guard`
- Intermediate `let` bindings that could be pipelined

**§4 Common Pitfalls:**
- `list.range` — deprecated, use `int.range`
- `===` — JavaScript habit, use `==`
- `Option` for fallible operations — use `Result(a, Nil)`

**§5 Conventions (from conventions.md C1-C10):**
- `import .* \.{` with functions/constants imported unqualified (C1)
- `pub fn` or `fn` without `-> ReturnType` (C2)
- `JSON`, `SQL`, `HTTP`, `URL` etc. in all-caps in names (C5)
- `_into_`, `_as_`, `_of_` in conversion function names instead of `_to_` (C6)
- Plural module names (C4)

**§6 Idioms (from conventions.md P1-P6):**
- Error types without context fields (P1)
- Records with `Bool` fields representing domain concepts (P4)

**§7 Anti-patterns (from conventions.md A1-A10):**
- Short variable names: `cap`, `off`, `cnt`, `proc`, `dat`, `ss`, `btn`, `lbl`, `idx`, `num`, `tmp`, `str`, `val`, `conf`, `cfg` (A1)
  - **Exceptions (do not flag):** `db`, `req`, `res`, `err`, `ctx`, `msg`, `id`, `fn`
- `types.gleam`, `constants.gleam`, `utilities.gleam` as separate files in a domain (A2)
- `controllers/`, `services/`, `models/`, `repositories/`, `helpers/`, `utils/` directories (A6)
- `result.is_ok` or `result.is_error` near `let assert` (A7)
- `Dynamic` type in non-test FFI code (A8)
- `_ ->` on custom type case expressions (A9)
- `Monoid`, `Functor`, `Catamorphism`, `Semigroup` abstractions (A10)

**§8 Logging & Validation:**
- Log levels: `error` vs `warning` vs `info` used correctly
- `configure_logger` called at startup
- No sensitive data in log messages
- Structured context, not string interpolation
- Typed validation errors, not plain strings
- `all` for multiple rules on one field, `then` for dependent validations

**§9 HTTP & Middleware (wisp):**
- Standard middleware stack: `rescue_crashes`, `log_request`, `handle_head`, `csrf_known_header_protection`
- `Signed` cookies for sessions
- Body size limits set
- `escape_html` for user content in HTML

**§10 Domain-Specific:**
- Backend: OTP actor patterns, SQL safety, S3 error handling
- Frontend: Lustre MVU, effects, routing

### Step 6: Report findings

#### Light Mode (default)

Organize findings by section. For each finding include:
- Severity tag: `[P0]`, `[P1]`, `[P2]`, or `[P3]`
- File path and line number: `file.gleam:42`
- What the issue is (one line)
- Suggested fix (brief code snippet or instruction)

End with a summary line:
```
Summary: X issues (P0: N, P1: N, P2: N, P3: N)
```

#### Thorough Mode (`--thorough`)

Use the full audit report format:

```markdown
# Gleam Code Review — Audit Report

**Date:** [ISO date]
**Scope:** [Full | Path: ... | Changed files]
**Files scanned:** [count]
**Mode:** Thorough

---

## Summary

| Severity             | Count |
| -------------------- | ----- |
| P0 — Correctness     | N     |
| P1 — Architecture    | N     |
| P2 — Maintainability | N     |
| P3 — Convention      | N     |

**Total violations:** N
**Overall health:** [Critical | Degraded | Healthy]

Health criteria:
- **Critical:** Any P0 violations, or >10 P1 violations
- **Degraded:** 1–10 P1 violations, or >20 P2 violations
- **Healthy:** No P0/P1, ≤20 P2, any number of P3

---

## §1 Type Design
[Violations or "No violations"]

## §2 Error Handling
[Violations or "No violations"]

## §3 Code Patterns
[Violations or "No violations"]

## §4 Common Pitfalls
[Violations or "No violations"]

## §5 Conventions
[Violations or "No violations"]

## §6 Idioms
[Violations or "No violations"]

## §7 Anti-patterns
[Violations or "No violations"]

## §8 Logging & Validation
[Violations or "No violations"]

## §9 HTTP & Middleware
[Violations or "No violations"]

## §10 Domain-Specific
[Violations or "No violations"]

---

## All Violations

### [V-001] [Severity: P1] [Category: §7 Anti-patterns]

**File:** `path/to/file.gleam:42`
**Rule violated:** A7 — Check-then-Assert
**Detection:** `result.is_ok()` followed by `let assert Ok()`

**What's wrong:**
[Explanation of architectural impact, not just restating the rule]

**Before:**
```gleam
[Current code]
```

**After:**
```gleam
[Fixed code — copy-paste ready]
```

---

[Continue for EVERY violation]

---

## Files Scanned

| Directory | Files | Violations |
|-----------|-------|------------|
| src/...   | N     | N          |

## Clean Files

[List files that passed all checks]
```

For each violation in thorough mode, include:
- Numbered ID (V-001, V-002, ...)
- Severity and category
- File path and line number
- Rule reference (convention ID or section name)
- What's wrong (explain the impact, not just the rule)
- Before/after code (copy-paste ready)

**Special detection: Nested boolean cases**

When reviewing code patterns, specifically check for:
- Nested `case` expressions (2+ levels) where inner cases match on `True`/`False`
- These patterns inside Result/Option match arms
- Report: location, nesting depth, and suggest extracting to function with `bool.guard`
- Categorize as **P2** severity

## Arguments

The user invoked this command with: $ARGUMENTS

If no arguments were provided, review all `.gleam` files in `src/`.
