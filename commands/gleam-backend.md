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

Before writing any code:

1. Read `gleam.toml` to understand dependencies and project config
2. Use Glob to find existing `.gleam` files in `src/`
3. Identify the project's conventions:
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

### Step 6: Verify

After implementation:

1. Run `gleam check` to verify the code compiles
2. Run `gleam format` to ensure consistent formatting
3. If the project has tests, run `gleam test`

## Arguments

The user invoked this command with: $ARGUMENTS
