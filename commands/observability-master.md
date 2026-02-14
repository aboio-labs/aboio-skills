---
description: Find unlogged error branches and add observability
argument-hint: [file-or-directory]
allowed-tools: [Read, Glob, Grep, Edit]
---

# Observability Master

Find every unlogged `Error` branch in modified files and add structured logging with context.

## Arguments

The user invoked this command with: $ARGUMENTS

## Instructions

### Step 1: Identify target files

If `$ARGUMENTS` specifies a file or directory, use that. Otherwise:

1. Run `git diff --name-only HEAD` to find recently modified `.gleam` files
2. If no git changes, ask the user which files to audit

### Step 2: Find unlogged errors efficiently

**Follow `references/token-efficiency.md` rules.** Do NOT read entire files. Instead:

1. Use Grep to find `Error(` patterns across target files
2. Use Grep to find existing `wisp.log_` calls
3. Cross-reference: Error branches WITHOUT nearby log calls are your targets
4. Read only specific line ranges (offset+limit) around unlogged Error branches

### Step 3: Find unlogged error branches

Search for these patterns where errors are returned WITHOUT logging:

#### Pattern 1: Direct Error returns
```gleam
Error(SomeError) -> Error(...)  // ❌ No log
```

#### Pattern 2: Result.try without logging the error path
```gleam
use value <- result.try(operation())  // Error path not logged
```

#### Pattern 3: Case statements propagating errors silently
```gleam
case db_query() {
  Ok(data) -> process(data)
  Error(e) -> Error(e)  // ❌ Silent propagation
}
```

#### Pattern 4: try in use expressions without logging
```gleam
use conn <- result.try(get_connection(pool))  // ❌ Connection error not logged
```

### Step 4: Determine correct logging approach

For each unlogged error, decide the log level:

- **`wisp.log_error`** — Unexpected failures, system errors, external service failures
  - Database connection lost
  - File system errors
  - Third-party API failures
  - Parsing errors for expected-format data

- **`wisp.log_warning`** — Expected errors, validation failures, user input problems
  - Validation failures (bad email, weak password)
  - Not found errors (user doesn't exist)
  - Permission denied (user lacks access)
  - Rate limit exceeded

- **`wisp.log_info`** — Normal operational events
  - Successful logins
  - Cache hits/misses
  - Background job completion

### Step 5: Add logging with context

For each unlogged error, add logging with `string.inspect(err)` for full error context:

#### Example 1: Direct Error return
```gleam
// BEFORE
Error(e) -> Error(e)

// AFTER
Error(e) -> {
  wisp.log_error("Failed to fetch user: " <> string.inspect(e))
  Error(e)
}
```

#### Example 2: Result.try with error logging
```gleam
// BEFORE
use user <- result.try(db.get_user(id))

// AFTER
use user <- result.try({
  case db.get_user(id) {
    Ok(u) -> Ok(u)
    Error(e) -> {
      wisp.log_error("Database error fetching user " <> id <> ": " <> string.inspect(e))
      Error(e)
    }
  }
})
```

#### Example 3: Multiple errors in case
```gleam
// BEFORE
case validate_and_save(user) {
  Ok(saved) -> Ok(saved)
  Error(ValidationError(msg)) -> Error(...)
  Error(DbError(e)) -> Error(...)
}

// AFTER
case validate_and_save(user) {
  Ok(saved) -> Ok(saved)
  Error(ValidationError(msg)) -> {
    wisp.log_warning("User validation failed: " <> msg)
    Error(...)
  }
  Error(DbError(e)) -> {
    wisp.log_error("Database error saving user: " <> string.inspect(e))
    Error(...)
  }
}
```

### Step 6: Check imports

After adding logging, verify the file imports `wisp` and `gleam/string`:

```gleam
import wisp
import gleam/string
```

If not present, add them at the top of the imports section.

### Step 7: Report findings

For each file audited, show:
- File path
- Unlogged error branches found (count)
- Logging added (count by level: error/warning/info)
- Lines modified

End with summary:
- Total files modified
- Total error branches now logged
- Breakdown by log level
- Recommendation: "Run `gleam check` to verify compilation"

## Logging Best Practices

- **Always use `string.inspect(err)`** for full error details (not just error messages)
- **Never log sensitive data** (passwords, tokens, PII)
- **Use structured context** — include IDs, operation names, relevant parameters
- **Log before returning** — capture the error at the point it's handled
- **Preserve the error** — don't change Error types, just add logging

## Focus

Audit **recently modified code only** unless explicitly told otherwise. This keeps observability improvements focused on new/changed logic.
