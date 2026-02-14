---
description: Simplify and refine Gleam code for clarity and maintainability
argument-hint: [file-or-directory]
allowed-tools: [Read, Glob, Grep, Edit]
---

# Code Simplifier

Simplify and refine recently modified Gleam code for clarity, consistency, and maintainability while preserving all functionality.

## Arguments

The user invoked this command with: $ARGUMENTS

## Instructions

### Step 1: Identify target files

If `$ARGUMENTS` specifies a file or directory, use that. Otherwise:

1. Run `git diff --name-only HEAD` to find recently modified `.gleam` files
2. If no git changes, ask the user which files to simplify

### Step 2: Read the target code efficiently

**Follow `references/token-efficiency.md` rules.** Do NOT read entire files. Instead:

1. Run `git diff -U10` on each target file to see changes with context
2. Use Grep to find simplification patterns (nested `case`, intermediate `let`, etc.)
3. Read only specific line ranges around matches (offset+limit)

### Step 3: Analyze for simplification opportunities

Check for these patterns in order of priority:

#### 1. Nested Results → Flattened with `use`
```gleam
// BEFORE
case fetch_user(id) {
  Ok(user) -> case validate_user(user) {
    Ok(valid_user) -> process(valid_user)
    Error(e) -> Error(e)
  }
  Error(e) -> Error(e)
}

// AFTER
use user <- result.try(fetch_user(id))
use valid_user <- result.try(validate_user(user))
process(valid_user)
```

#### 2. Repeated function calls → Pipeline
```gleam
// BEFORE
string.trim(string.lowercase(string.replace(input, "_", "-")))

// AFTER
input
|> string.replace("_", "-")
|> string.lowercase
|> string.trim
```

#### 3. Intermediate bindings → Inline
```gleam
// BEFORE
let doubled = x * 2
let result = doubled + 10
result

// AFTER
x * 2 + 10
```

#### 4. Explicit recursion → stdlib function
```gleam
// BEFORE
fn sum(list: List(Int), acc: Int) -> Int {
  case list {
    [] -> acc
    [x, ..rest] -> sum(rest, acc + x)
  }
}

// AFTER
list.fold(list, 0, fn(acc, x) { acc + x })
```

#### 5. Case on bools → if/else or guards
```gleam
// BEFORE
case is_valid {
  True -> "yes"
  False -> "no"
}

// AFTER
if is_valid { "yes" } else { "no" }
```

#### 6. Unnecessary string building → direct concatenation
```gleam
// BEFORE
string_builder.from_string("Hello")
|> string_builder.append(", ")
|> string_builder.append(name)
|> string_builder.to_string

// AFTER
"Hello, " <> name
```

#### 7. Nested boolean cases inside pattern match → Extract with guards
```gleam
// BEFORE
case fetch_user(id) {
  Ok(user) -> {
    case user.is_verified {
      True -> case user.is_active {
        True -> show_dashboard(user)
        False -> Error("Account inactive")
      }
      False -> Error("Account not verified")
    }
  }
  Error(e) -> Error(e)
}

// AFTER
case fetch_user(id) {
  Ok(user) -> validate_user_access(user)
  Error(e) -> Error(e)
}

fn validate_user_access(user: User) -> Result(Page, String) {
  use <- bool.guard(!user.is_verified, Error("Account not verified"))
  use <- bool.guard(!user.is_active, Error("Account inactive"))
  Ok(show_dashboard(user))
}
```

#### 8. Project-specific helpers
Check if the project has helpers in `helper/` or `shared/` that replace manual patterns:
- `helper/error/response.gleam` → `error_response.handle(err)`
- `helper/http/params.gleam` → `get_page_params(req)`
- `helper/db/transaction.gleam` → `run_in_transaction(db, fn)`

### Step 4: Apply simplifications

For each file with opportunities:

1. Use Edit tool to apply transformations
2. Preserve exact indentation (tabs/spaces)
3. Keep all comments
4. DO NOT change behavior
5. DO NOT add type annotations unless already present
6. DO NOT add error handling where it doesn't exist

### Step 5: Report changes

For each simplified file, show:
- File path
- What was simplified (bullet list)
- Lines changed count

End with summary:
- Total files modified
- Total lines simplified
- Recommendation: "Run `gleam check` to verify compilation"

## Safety Rules

- **NEVER** change function signatures
- **NEVER** add error handling that wasn't there
- **NEVER** remove error handling that was there
- **NEVER** change public APIs
- **NEVER** modify test expectations
- Only simplify — don't add features

## Focus

Simplify **recently modified code only** unless explicitly told otherwise. This keeps simplification relevant to current work.
