---
description: Audit OTP actors, supervision trees, and concurrent state management
argument-hint: <files-or-scope>
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit]
---

# OTP Architecture Review

Audit Gleam/OTP code for actor design, supervision strategy, concurrency safety, and state management patterns. Use after implementing or modifying actors, supervision trees, or concurrent state.

## When to Use

**Always use after:**
- Creating new actors
- Modifying supervision trees
- Changing actor state or message types
- Adding concurrent state management
- Modifying kafka.gleam startup

**Also use when:**
- Actor code is growing complex (>200 lines)
- Experiencing timeouts, deadlocks, or memory leaks
- Considering ETS migration for performance
- Unclear supervision strategy needed

## Instructions

### Step 1: Identify OTP-related changes

Find files with OTP code:

```bash
# If reviewing uncommitted work
git status

# If reviewing recent commit
git diff --name-only HEAD~1
```

Focus on files containing: `actor.`, `supervisor.`, `factory.`, `process.`, `Subject(`, `StartResult`.

### Step 2: Load relevant references

**Always load (foundational):**

- `~/.claude/skills/gleam/references/backend/otp.md`
- `~/.claude/skills/gleam/references/backend/otp-supervision.md`
- `~/.claude/skills/gleam/references/fundamentals/error-handling.md`
- `~/.claude/skills/gleam/references/fundamentals/code-patterns.md`

**Load based on complexity:**

| Pattern | Reference |
|---------|-----------|
| Advanced selectors, timers, ETS | `references/backend/otp-advanced.md` |
| Logging actor failures | `references/backend/logging.md` |
| Three-tier error handling | `references/backend/three-tier-error-handling.md` |

All backend references live under `~/.claude/skills/gleam/references/backend/`.

### Step 3: Run the OTP Audit Checklist

For each file with OTP code, verify:

#### Actor Design

- [ ] **Client API exists** — Callers NEVER construct Message variants directly
  - ✓ Each message has a corresponding public function
  - ✓ Functions accept business types, construct messages internally
  - ✗ Anti-pattern: `process.send(actor, GetUser(id, reply_subject))`
  - ✓ Pattern: `get_user(actor, id) -> User`

- [ ] **Request-reply pattern** — Sync operations include `Subject` in reply variant
  - ✓ Pattern: `GetSettings(reply_to: Subject(Result(Settings, Error)))`
  - ✗ Anti-pattern: `GetSettings(id: String)` with no reply mechanism

- [ ] **State type well-defined** — Record with named fields
  - ✓ `pub type State { State(cache: Dict(String, Value), ttl_ms: Int) }`
  - ✗ Anti-pattern: `Dict(String, String)` as raw state

- [ ] **Handlers return actor.continue()** — All non-stop paths
  - ✓ `actor.continue(new_state)`
  - ✗ Missing return value causes compilation error

- [ ] **No blocking operations** — Spawn for heavy work
  - ✗ Anti-pattern: `sql.query(db, ...)` inside handle_message (blocks other messages)
  - ✓ Pattern: `process.start(fn() { ... })` or queue message

- [ ] **Timeout is 5000ms** — Project convention for actor.call
  - ✓ `actor.call(subject, 5000, fn(reply) { ... })`
  - ✗ `actor.call(subject, 1000, ...)` without justification

- [ ] **Start returns Result** — `Result(Subject(Message), actor.StartError)` or `actor.StartResult`
  - ✓ Type signature matches gleam_otp v1.2.0
  - ✗ Returning bare `Subject` without error handling

- [ ] **No try_call pattern** — Removed from gleam_erlang (memory leak)
  - ✗ Anti-pattern: `process.try_call(subject, ...)` (doesn't exist in modern gleam_otp)
  - ✓ Use `actor.call` which panics on timeout by design

#### Supervision

- [ ] **Every actor is supervised** — Not bare `let assert Ok()`
  - ✗ Anti-pattern: `let assert Ok(cache) = cache.start()` in kafka.gleam
  - ✓ Pattern: Add to static supervisor

- [ ] **Strategy matches failure mode**
  - `OneForOne` — Independent children (default)
  - `OneForAll` — Shared-state children (restart all when one fails)
  - `RestForOne` — Sequential dependencies (restart dependents)

- [ ] **restart_tolerance configured** — Not just defaults
  - ✓ `supervisor.restart_tolerance(builder, intensity: 3, period: 5)`
  - Means: Allow 3 restarts in 5 seconds before giving up

- [ ] **Start functions return Result(Started(data), StartError)**
  - ✓ Matches supervision.worker/supervisor spec signature

- [ ] **process.sleep_forever() in main** — Or supervisor keeps VM alive
  - ✓ Last line of kafka.gleam main function

- [ ] **Restart strategy appropriate**
  - `Permanent` — Long-lived services (cache, store, rate limiter)
  - `Transient` — Tasks that naturally complete (HTTP requests, batch jobs)
  - `Temporary` — Fire-and-forget (logging, metrics)

- [ ] **Shutdown timeout appropriate** — Default 5000ms
  - Increase if cleanup needs more time (flush buffers, close connections)

#### State Management

- [ ] **State growth is bounded** — Cleanup/TTL for time-based data
  - ✗ Anti-pattern: Dict grows forever, OOM after days/weeks
  - ✓ Patterns:
    - Periodic cleanup: `process.send_after(self, 60_000, Cleanup)`
    - TTL per entry: Track insertion time, remove expired
    - Max size: Remove oldest when size > threshold

- [ ] **Critical state persisted to database** — Not just actor memory
  - ✗ Anti-pattern: User sessions only in actor (lost on restart)
  - ✓ Pattern: Actor as cache, DB as source of truth

- [ ] **No duplicate state across actors** — Single source of truth
  - ✗ Anti-pattern: Store and Cache both maintain user preferences
  - ✓ Pattern: Cache reads from Store, Store owns state

- [ ] **High-read state evaluated for ETS** — Only if profiling shows bottleneck
  - ⚠️ Don't prematurely optimize — Dict is fast enough for most workloads
  - ✓ Use ETS only if profiling shows actor is bottleneck (>10k reads/sec)

- [ ] **Dict keys appropriate type** — String for IDs, not complex records
  - ✓ `Dict(String, User)` for user_id → User
  - ✗ `Dict(#(String, Int), Value)` (tuple keys hard to query)

#### Concurrency Safety

- [ ] **No self-call deadlock** — Actor calling itself via actor.call
  - ✗ Anti-pattern: `get_user(actor, id) { actor.call(actor, ...) }` from within handle_message
  - ✓ Pattern: Direct state access inside handle_message

- [ ] **No Subject ownership violations** — Receiving on subject created in another process
  - ✗ Anti-pattern: Passing `process.new_subject()` across processes
  - ✓ Pattern: Only owner receives on subject

- [ ] **process.Name created ONCE** — Atoms never GC'd
  - ✗ Anti-pattern: `process.new_name("cache_" <> id)` in loop (leaks atoms)
  - ✓ Pattern: `process.new_name("tenant_cache")` at module level

- [ ] **No circular sync call chains** — A calls B calls A = deadlock
  - ✗ Anti-pattern: Store calls Cache.get, Cache calls Store.validate
  - ✓ Pattern: Async messaging, shared read-only state, or single direction

- [ ] **process.sleep_forever() in main** — Prevents VM exit
  - ✓ Last line of kafka.gleam main function

#### Error Handling

- [ ] **Actor start failures logged** — wisp.log_error
  - ✓ Pattern: `Error(e) -> { wisp.log_error(...); Error(e) }`
  - ✗ Silent failures make debugging impossible

- [ ] **Message handler errors don't crash** — Return actor.continue with error state
  - ✓ Pattern: `case operation() { Error(e) -> actor.continue(State(..state, error: Some(e))) }`
  - ⚠️ Only use `actor.stop_abnormal` when actor is truly corrupted

- [ ] **Client functions document timeout** — Panics by design via actor.call
  - ✓ Docstring: `/// Panics if actor doesn't respond within 5 seconds`

- [ ] **Supervisor start failures handled** — Not just let assert in production
  - ✗ Anti-pattern: `let assert Ok(sup) = supervisor.start(builder)`
  - ✓ Pattern: `case supervisor.start(builder) { Ok(sup) -> ..., Error(e) -> wisp.log_error(...) }`

#### Process Naming

- [ ] **Named actors use actor.named** — Never process.register()
  - ✓ `actor.named(builder, process.new_name("cache"))`
  - ✗ `process.register(subject, name)` after start

- [ ] **process.new_name() called once** — At module/startup level
  - ✓ `const cache_name = process.new_name("cache")` at module top
  - ✗ `process.new_name("cache")` inside function (creates new atom each call)

- [ ] **Named factory supervisors** — factory.named
  - ✓ `factory.named(builder, process.new_name("workers"))`

### Step 4: Check kafka.gleam startup

Always review `server/src/kafka.gleam`:

```gleam
// Anti-pattern: Unsupervised actors
let assert Ok(cache) = cache.start()
let assert Ok(store) = store.start()

// Pattern: Static supervisor
let assert Ok(supervisor) = {
  use builder <- result.try(supervisor.new(supervisor.OneForOne))

  builder
  |> supervisor.restart_tolerance(intensity: 3, period: 5)
  |> supervisor.add(cache.supervised())
  |> supervisor.add(store.supervised())
  |> supervisor.start()
}
```

Verify:
- All actors supervised?
- Startup order correct? (Dependencies before dependents)
- StartError handled? (Not just `let assert Ok()`)
- `process.sleep_forever()` present?

### Step 5: Evaluate state patterns

For each actor with Dict-based state:

**Ask:**
- What happens after 1M entries?
- Is there cleanup? (TTL, periodic purge, max size)
- Should this be in database instead?
- Is read concurrency a concern? (Probably not at this scale)

**Common patterns:**

```gleam
// TTL-based cleanup
pub type State {
  State(
    cache: Dict(String, #(Value, Int)),  // value + timestamp
    ttl_ms: Int,
  )
}

fn cleanup_expired(state: State) -> State {
  let now = now_ms()
  let cache = dict.filter(state.cache, fn(_, entry) {
    let #(_, timestamp) = entry
    now - timestamp < state.ttl_ms
  })
  State(..state, cache: cache)
}

// Scheduled cleanup in init
actor.selecting(init, process.selecting_system_messages(
  process.new_selector(),
  fn(msg) {
    case msg {
      process.System(process.SelfMessage(Cleanup)) -> {
        process.send_after(self, 60_000, Cleanup)
        cleanup_expired(state)
        |> actor.continue()
      }
      _ -> actor.continue(state)
    }
  }
))
```

### Step 6: Fix or flag issues

**Fix directly:**
- Missing client API functions
- Missing `wisp.log_error` on start failures
- Obvious `process.new_name()` duplication
- Missing `actor.continue()` returns

**Flag for user (architectural decisions):**
- Splitting actors (needs design discussion)
- Adding supervision (needs startup modification)
- Changing strategies (needs failure mode analysis)
- Adding dependencies (bravo for ETS, chip for registry)

### Step 7: Verify compilation

After making changes:

```bash
cd server && gleam check
cd server && gleam format
```

**Never leave the codebase broken.**

## Output Format

Provide clear audit results:

```markdown
## OTP Audit Report

### Files Reviewed
- `src/actors/cache.gleam` (157 lines)
- `src/kafka.gleam` (startup)

### Issues Fixed
1. `src/actors/cache.gleam:42` — Added client API function `get_setting(cache, key)`
2. `src/actors/cache.gleam:15` — Added `wisp.log_error` on actor start failure
3. `src/kafka.gleam:28` — Replaced bare start with supervised start

### Design Recommendations

**[CRITICAL] Unbounded State Growth**
- `cache.gleam` Dict grows without cleanup
- After 1M entries, VM will OOM
- **Recommend:** Add TTL-based cleanup with `process.send_after`
- **Reference:** `~/.claude/skills/gleam/references/backend/otp-advanced.md` (section: State Cleanup)

**[HIGH] Missing Supervision**
- Store and RateLimiter start unsupervised in kafka.gleam
- **Recommend:** Add static supervisor
- **Reference:** `~/.claude/skills/gleam/references/backend/otp-supervision.md`

**[MEDIUM] Suboptimal Message Pattern**
- `GetSettings` uses fire-and-forget (no reply channel)
- Callers can't detect failure
- **Recommend:** Add `Subject(Result(Settings, Error))` to message variant

### Checklist Results
- ✓ Actor Design: 6/8 (missing: client API for 2 messages)
- ✗ Supervision: 2/8 (actors unsupervised)
- ✓ Concurrency Safety: 5/5
- ✓ Error Handling: 4/5 (missing: start failure logging)
- ✓ State Management: 3/5 (unbounded growth, no TTL)

### Compilation
✓ `gleam check` — Pass
✓ `gleam format` — Applied
```

## Integration with gleam-backend

**When implementing features with OTP:**

1. Use `/gleam-backend` to implement the feature
2. Use `/tdd` to write tests first
3. **Use this command** (`/otp-review`) after implementation
4. Fix issues found by review
5. Run `/observability-master` to add logging
6. Run `/code-simplifier` for final cleanup

**Example workflow:**

```bash
# User: "Add a tenant settings cache actor"

1. /gleam-backend "tenant settings cache actor"
   → Implements actor with messages, state, handlers

2. /tdd (automatically called by gleam-backend)
   → RED tests for cache operations
   → GREEN implementation

3. /otp-review src/actors/settings_cache.gleam src/kafka.gleam
   → Audits actor design, supervision, state management
   → Fixes client API, adds supervision

4. /observability-master
   → Adds logging for start failures, message handling errors

5. /code-simplifier
   → Cleans up redundant bindings
```

## Common Patterns & Anti-Patterns

### Client API Pattern

```gleam
// ✓ CORRECT - Client API
pub fn get_user(cache: Subject(Message), id: String) -> Result(User, Error) {
  actor.call(cache, 5000, fn(reply) {
    GetUser(id:, reply:)
  })
}

// ✗ WRONG - Direct message construction
process.send(cache, GetUser(id: "123", reply: my_subject))
```

### Supervision Pattern

```gleam
// ✓ CORRECT - Supervised start
pub fn supervised() -> ChildSpecification(Subject(Message)) {
  supervision.worker(start)
  |> supervision.restart(supervision.Permanent)
  |> supervision.timeout(5000)
}

// In kafka.gleam
supervisor.add(builder, cache.supervised())

// ✗ WRONG - Bare start
let assert Ok(cache) = cache.start()
```

### State Cleanup Pattern

```gleam
// ✓ CORRECT - Bounded state with TTL
pub type State {
  State(
    entries: Dict(String, Entry),
    max_size: Int,
    ttl_ms: Int,
  )
}

pub type Entry {
  Entry(value: Value, inserted_at: Int)
}

fn cleanup(state: State) -> State {
  let now = birl.to_unix(birl.now())
  let entries =
    dict.filter(state.entries, fn(_, entry) {
      now - entry.inserted_at < state.ttl_ms
    })
    |> dict.take(state.max_size)  // Limit size

  State(..state, entries:)
}

// ✗ WRONG - Unbounded growth
pub type State = Dict(String, Value)
// No cleanup, no max size, no TTL
```

### Error Handling Pattern

```gleam
// ✓ CORRECT - Logged start failure
pub fn start() -> Result(Subject(Message), actor.StartError) {
  actor.new(initial_state())
  |> actor.on_message(handle_message)
  |> actor.start()
  |> result.map_error(fn(err) {
    wisp.log_error("Failed to start cache actor: " <> string.inspect(err))
    err
  })
}

// ✗ WRONG - Silent failure
pub fn start() -> Result(Subject(Message), actor.StartError) {
  actor.new(initial_state())
  |> actor.on_message(handle_message)
  |> actor.start()
  // No logging on Error path
}
```

## Behavioral Guidelines

- **Be specific** — Don't say "add supervision", show the exact code or reference the pattern
- **Distinguish severity** — Missing supervisor is CRITICAL, suboptimal Dict key is MEDIUM
- **Respect project scale** — Publishing ERP, not HFT. Don't recommend ETS unless profiling justifies
- **Reference skill docs** — Point to specific sections in otp.md, otp-supervision.md, otp-advanced.md
- **Never modify generated code** — Skip `sql.gleam`, `.sql` files
- **Verify compilation** — Always run `gleam check` after changes
- **Don't over-engineer** — If Dict + moderate traffic works, don't add ETS/registries/dynamic supervisors

## Quick Decision Tree

```
OTP code changed?
├─ Actor created/modified → Check Actor Design + State Management
├─ Supervision changed → Check Supervision + Startup Order
├─ kafka.gleam modified → Check all actors supervised + sleep_forever
├─ State growing → Evaluate cleanup pattern + DB migration
└─ Timeout/deadlock → Check Concurrency Safety
```

## Arguments

The user invoked this command with: $ARGUMENTS
