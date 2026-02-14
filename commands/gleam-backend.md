---
description: Build a Gleam backend feature (endpoint, handler, SQL, OTP)
argument-hint: <feature-description>
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit]
---

# Gleam Backend Feature Builder

Build a backend feature following Gleam best practices — endpoints, handlers, database queries, OTP actors, auth, and more.

## Instructions

### Step 1: Understand the request

The user wants to build: **$ARGUMENTS**

Parse what kind of backend feature this involves. It may include one or more of:

- HTTP endpoint/handler
- Database query (SQL)
- Authentication/authorization
- OTP actor/process
- Background task/effect
- Logging

### Step 2: Load relevant references

Based on the feature type, read the appropriate reference files:

**Always load (foundational):**

- `~/.claude/skills/gleam/references/fundamentals/type-design.md`
- `~/.claude/skills/gleam/references/fundamentals/decoding.md`
- `~/.claude/skills/gleam/references/fundamentals/code-patterns.md`
- `~/.claude/skills/gleam/references/fundamentals/error-handling.md`

**Load based on feature type:**

| Feature               | Reference                                     |
| --------------------- | --------------------------------------------- |
| Web framework (wisp)  | `references/backend/wisp-framework.md`        |
| HTTP server (mist)    | `references/backend/mist-server.md`           |
| HTTP endpoint/handler | `references/backend/http-runner.md`           |
| Database/SQL          | `references/backend/squirrel-guide.md`        |
| JWT auth              | `references/backend/jwt-ywt.md`               |
| Password/time auth    | `references/backend/auth.md`                  |
| OTP actor/process     | `references/backend/otp.md`                   |
| Logging               | `references/backend/logging.md`               |
| Algebraic effects     | `references/backend/midas-effect-task.md`     |
| S3 / object storage   | `references/backend/bucket-s3.md`             |
| Input validation      | `references/fundamentals/validation-valid.md` |

All backend references live under `~/.claude/skills/gleam/references/backend/`.

### Step 3: Explore existing project structure

**Follow `references/token-efficiency.md` rules.** Explore surgically, not exhaustively.

Before writing any code:

1. Grep `gleam.toml` for specific dependencies you need (e.g., `wisp`, `pog`, `squirrel`)
2. Use Glob to find existing `.gleam` files matching your feature area (not all of `src/`)
3. Identify the project's conventions using targeted Grep queries:
   - Router/handler organization
   - Module naming patterns
   - Where types are defined
   - Error type patterns
   - Database query file locations (if using Squirrel)

### Step 4: Call relevant skills

Before writing any code:

1. Call `/tdd` skills to guide you through implementation;
2. Call `/workflow-orchestration` skill to create a plan and the tools needed;
3. Call `/using-git-worktress` skill to handle the feature in a new worktree.
4. Call `/security-auditor` skill after each implementation for making sure you are not developing bugs in our project;
5. Call `api-designer` to give you guidance on how to create the feature.

### Step 5: Implement the feature

Follow the patterns from the loaded references:

- Define types with opaque wrappers where appropriate
- Use `Result` for all fallible operations
- Follow the project's existing module structure
- Use `use` expressions for Result chaining
- Apply label shorthand syntax where it improves clarity
- Add proper error variants to the project's error type
- Never use `let _ =`. This is an anti-pattern

**Security Checkpoint: Dynamic Table Names**

If your SQL queries use dynamic table names (cannot be parameterized):
1. ALWAYS validate against a compile-time allowlist before concatenation
2. NEVER pass raw strings into SQL, even in test code
3. Use panic for invalid table names (compile-time enforcement)

```gleam
// Example: RLS test helper
const allowed_rls_tables = [
  "orders", "products", "customers", "invoices"
]

fn validate_table_name(table: String) -> String {
  case list.contains(allowed_rls_tables, table) {
    True -> table
    False -> panic as "Invalid table name: " <> table
  }
}

// Use in SQL concatenation
let table = validate_table_name(user_provided_table)
let query = "SELECT * FROM tenant." <> table <> " WHERE id = $1"
```

This prevents SQL injection even in test-only code and establishes safe patterns.

### Step 6: Verify

After implementation:

1. Run `gleam check` to verify the code compiles
2. Run `gleam format` to ensure consistent formatting
3. If the project has tests, run `gleam test`

**Testing Guidance: ValidationError HTTP Status**

When writing tests for validation failures, remember:
- `error.ValidationErr` → **400 Bad Request** (from centralized error handler)
- `wisp.unprocessable_content()` → **422 Unprocessable Entity** (only for JSON decode errors)

```gleam
// Correct test expectations
pub fn rejects_invalid_email_test() {
  let req = simulate.post("/api/users", [], "{\"email\":\"not-an-email\"}")
  let response = router.handle(req, ctx.db)

  response.status |> should.equal(400)  // ValidationErr → 400
}

pub fn rejects_malformed_json_test() {
  let req = simulate.post("/api/users", [], "{bad json")
  let response = router.handle(req, ctx.db)

  response.status |> should.equal(422)  // JSON decode error → 422
}
```

### Step 7: Review Agent Cleanup

After running review agents (code-simplifier, observability-master, etc.):

1. Check which files were modified:
   ```bash
   git diff --name-only
   ```

2. Revert changes to files **outside** your feature scope:
   ```bash
   git checkout -- path/to/unrelated/file.gleam
   ```

3. Only commit changes directly related to your feature

**Why this matters:** Review agents sometimes modify files outside the current task scope. These changes should be reverted before committing to keep the PR focused and reviewable.

**Example:**
```bash
# Working on gleam-backend feature, code-simplifier modifies:
# - src/backend/handler.gleam (your feature) ✓ keep
# - src/frontend/component.gleam (unrelated) ✗ revert
# - lib/validation.gleam (unrelated) ✗ revert

git status
# On branch feature/backend-endpoint
# Changes not staged:
#   modified: src/backend/handler.gleam
#   modified: src/frontend/component.gleam
#   modified: lib/validation.gleam

git checkout -- src/frontend/component.gleam lib/validation.gleam
git add src/backend/handler.gleam
git commit -m "feat: add backend endpoint for orders"
```

## Arguments

The user invoked this command with: $ARGUMENTS
