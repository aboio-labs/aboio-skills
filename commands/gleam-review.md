---
description: Review Gleam code against best practices
argument-hint: [file-or-directory]
allowed-tools: [Read, Glob, Grep]
---

# Gleam Code Review

Review Gleam code against the full best practices reference — fundamentals, error handling, type design, and domain-specific patterns.

## Instructions

### Step 1: Load core references

Read these foundational reference files:

- `~/.claude/skills/gleam/references/fundamentals/common-pitfalls.md`
- `~/.claude/skills/gleam/references/fundamentals/error-handling.md`
- `~/.claude/skills/gleam/references/fundamentals/type-design.md`
- `~/.claude/skills/gleam/references/fundamentals/code-patterns.md`

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

### Step 4: Scan the target code

If `$ARGUMENTS` specifies a file or directory, scan that. Otherwise, scan all `.gleam` files in `src/`.

Use Glob to find `.gleam` files, then Read each one.

### Step 5: Report findings

Organize the review into these categories:

1. **Type Design** — opaque types, parse-don't-validate, phantom types
2. **Error Handling** — Result usage, error propagation, `let assert` usage
3. **Code Patterns** — use expressions, pipelines, label usage, dead code
4. **Common Pitfalls** — string building, list prepend vs append, imports, naming
5. **Logging** — correct log levels (error vs warning vs info), `configure_logger` called at startup, no sensitive data in log messages, using structured context not string interpolation
6. **Validation** — typed errors not plain strings, separate input/output types (parse don't validate), `all` for multiple rules on one field, `then` for dependent validations, `optional` for nullable fields
7. **HTTP & Middleware** (wisp) — standard middleware stack present (`rescue_crashes`, `log_request`, `handle_head`, `csrf_known_header_protection`), `Signed` cookies for sessions, body size limits set, `escape_html` for user content in HTML
8. **Domain-Specific** — backend (OTP patterns, SQL safety, S3 error handling) or frontend (Lustre MVU, effects, routing)

For each finding, include:
- The file path and line number (`file.gleam:42`)
- What the issue is
- What the best practice recommends
- A suggested fix (code snippet)

End with a summary: total issues found, grouped by severity (error, warning, suggestion).

## Arguments

The user invoked this command with: $ARGUMENTS

If no arguments were provided, review all `.gleam` files in `src/`.
