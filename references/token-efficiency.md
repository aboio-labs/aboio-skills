# Token-Efficient Agent Strategy

**Every skill, command, and agent in this repository MUST follow these rules to minimize token consumption.** Reading entire files when targeted patterns suffice wastes 10-20k tokens per file. Surgical, pattern-focused approaches achieve the same results with 80-90% fewer tokens.

## The Five Rules

### Rule 1: Start with git diff to scope work

Always begin by identifying what changed. Never read entire directories or all project files.

```bash
# DO: Scope to what changed
git diff --stat                                    # File list + line counts (~200 tokens)
git diff --name-only                               # Changed files only (~100 tokens)
git diff --name-only | grep -E "handler|service"   # Only relevant files (~50 tokens)

# DON'T: Read everything
Read src/tenant/*/handler.gleam                    # Could be 15k+ tokens per file
Glob **/*.gleam | Read each                        # Could be 100k+ tokens
```

**When to use:** At the start of every task that operates on "recently modified" or "changed" code.

### Rule 2: Grep for patterns instead of reading files

Use Grep to find specific patterns directly. Don't read files to search for something.

```bash
# DO: Find Error branches without logging
Grep "Error(" --type gleam                          # Find all Error returns (~500 tokens)
Grep "Error.*->.*Error" --type gleam                # Find error propagation (~300 tokens)
Grep "wisp.log_" --type gleam                       # Find existing logging (~200 tokens)

# DON'T: Read all files and scan manually
Read src/handler.gleam                              # 5k tokens to find 3 relevant lines
Read src/service.gleam                              # 8k tokens to find 2 relevant lines
```

**When to use:** Whenever you need to find specific code patterns, imports, function calls, or error handling across files.

### Rule 3: Read only specific line ranges around matches

After Grep identifies locations, read only the surrounding context — not the entire file.

```bash
# DO: Read targeted ranges
Grep "Error(" src/handler.gleam -n                  # Get line numbers
Read src/handler.gleam offset=42 limit=20           # Read 20 lines around match

# DON'T: Read the entire file
Read src/handler.gleam                              # 300 lines = 5k tokens
                                                    # when you only needed lines 42-62
```

**When to use:** After any Grep or search identifies specific locations. Always prefer offset+limit over full file reads.

### Rule 4: Use diff context for changed functions only

When reviewing changes, use git diff with context rather than reading full files.

```bash
# DO: See exactly what changed with context
git diff -U10 src/handler.gleam                     # Changed lines + 10 lines context
git diff HEAD~1 -- src/handler.gleam                # Changes from last commit

# DON'T: Read the entire file to understand changes
Read src/handler.gleam                              # Full file when only 15 lines changed
```

**When to use:** When auditing, reviewing, or simplifying code that was recently modified.

### Rule 5: Check critical files with targeted queries

For known critical files (routers, configs, startup), use targeted Grep queries rather than full reads.

```bash
# DO: Check specific concerns
Grep "process.sleep_forever" src/kafka.gleam        # Verify VM stays alive
Grep "supervisor.add" src/kafka.gleam               # Check supervised actors
Grep "pog.transaction" src/                         # Find transaction nesting

# DON'T: Read full critical files every time
Read src/kafka.gleam                                # 200 lines when checking 1 pattern
Read gleam.toml                                     # 50 lines when checking 1 dependency
```

**When to use:** When verifying specific properties of known files (startup, config, routing).

## Token Budget Examples

| Task | Inefficient Approach | Efficient Approach | Savings |
|------|--------------------|--------------------|---------|
| Find unlogged errors | Read 10 handlers (50k tokens) | Grep `Error` + read ranges (3k tokens) | **94%** |
| Audit OTP actors | Read all actor files (30k tokens) | Grep patterns + targeted reads (4k tokens) | **87%** |
| Review SQL safety | Read all .sql files (20k tokens) | Grep `$` params + read ranges (2k tokens) | **90%** |
| Simplify code | Read all changed files fully (40k tokens) | git diff + targeted reads (5k tokens) | **88%** |
| Check RLS policies | Read all migrations (25k tokens) | Grep `POLICY\|RLS\|FORCE` (1k tokens) | **96%** |

## Anti-Patterns

These waste tokens and MUST be avoided:

1. **"Read the file to understand it"** — Use Grep to find what you need, then read targeted ranges
2. **"Read each target file completely"** — Use git diff for changed files, Grep for pattern matching
3. **"Use Glob to find files, then Read each one"** — Use Grep with file type filters instead
4. **"Load each file to check for X"** — Use Grep for X directly across all files
5. **"Read gleam.toml to understand dependencies"** — Grep for the specific dependency you need
6. **"Scan all .gleam files in src/"** — Grep for the specific pattern in src/

## When Full File Reads Are Acceptable

Full reads are justified ONLY when:

1. **Writing new code** that must integrate with an existing file's structure (read once, understand layout)
2. **The file is small** (<50 lines) and you need most of its content
3. **You need to edit the file** and must understand the full context for safe changes
4. **Reference files** from skills that are designed to be read in full (e.g., skill reference docs)
5. **First encounter** with a project — read key files once to understand structure, then switch to targeted reads

## Integration

Every command and skill in this repository should:

1. **Start with scoping** — What files changed? What am I looking for?
2. **Use Grep first** — Find patterns before reading files
3. **Read targeted ranges** — Use offset+limit after finding matches
4. **Avoid redundant reads** — Don't re-read files you've already examined
5. **Track token budget** — If a task is consuming >10k tokens on file reads, reassess approach
