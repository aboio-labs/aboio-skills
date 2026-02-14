---
description: Test-Driven Development - write failing test first, then implement
argument-hint: <feature-description>
allowed-tools: [Read, Glob, Grep, Write, Edit, Bash]
---

# Test-Driven Development (TDD)

Apply strict TDD cycle: RED → GREEN → REFACTOR. Never write production code without a failing test first.

## Arguments

The user invoked this command with: $ARGUMENTS

## Core Principle

**NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST**

If you didn't watch the test fail, you don't know if it tests the right thing.

## Instructions

### Step 0: Load TDD Reference

**Follow `references/token-efficiency.md` rules.** Use targeted reads and Grep throughout.

Read the TDD skill to understand the cycle:
- Core principles and iron laws
- Red-Green-Refactor cycle
- Common rationalizations to avoid
- Testing anti-patterns

### Step 1: Understand What to Build

Based on `$ARGUMENTS`, identify:
- What behavior needs to be added or fixed
- What the expected outcome is
- What edge cases might exist

Ask clarifying questions if unclear.

### Step 2: RED - Write Failing Test

**BEFORE any implementation code:**

1. Create or locate the test file (Gleam convention: `test/<module>_test.gleam`)
2. Write ONE minimal test showing desired behavior
3. Test should be:
   - Clear name describing what it tests
   - Minimal - one behavior only
   - Using real code (avoid mocks unless necessary)

**Gleam test example:**
```gleam
import gleeunit/should
import myapp/user

pub fn validates_email_format_test() {
  user.create("", "password")
  |> should.be_error

  user.create("invalid-email", "password")
  |> should.be_error

  user.create("valid@email.com", "password")
  |> should.be_ok
}
```

### Step 3: Verify RED - Watch It Fail

**MANDATORY - NEVER SKIP**

Run the test and confirm:
```bash
cd server && gleam test
```

Check that:
- ✓ Test **fails** (not errors)
- ✓ Failure message makes sense
- ✓ Fails because **feature is missing**, not because of typos

**If test passes:** You're testing existing behavior - fix the test
**If test errors:** Fix the error and re-run until it fails correctly

### Step 4: GREEN - Write Minimal Code

Write the **simplest possible code** to make the test pass.

**DO:**
- Just enough to pass the test
- Hard-code if only one test case exists
- Focus on making it work first

**DON'T:**
- Add extra features "while you're at it"
- Refactor other code
- Add configuration options not tested
- Design for future requirements (YAGNI)

**Gleam implementation example:**
```gleam
pub fn create(email: String, password: String) -> Result(User, Error) {
  // Just enough to pass
  case email {
    "" -> Error(EmptyEmail)
    _ -> {
      case string.contains(email, "@") {
        False -> Error(InvalidEmail)
        True -> Ok(User(email, password))
      }
    }
  }
}
```

### Step 5: Verify GREEN - Watch It Pass

**MANDATORY**

Run the full test suite:
```bash
cd server && gleam test
```

Confirm:
- ✓ New test **passes**
- ✓ All other tests still pass
- ✓ No warnings or errors in output
- ✓ `gleam check` passes

**If test fails:** Fix the code, not the test
**If other tests fail:** Fix them now before continuing

### Step 6: REFACTOR - Clean Up (Optional)

**Only after tests are green:**

- Remove duplication
- Improve names
- Extract helper functions
- Apply patterns from code-simplifier skill

**Keep tests green** - re-run after each refactor step.

**Don't:**
- Add new behavior
- Change function signatures without updating tests
- Refactor unrelated code

### Step 7: Repeat for Next Behavior

If more behaviors needed for `$ARGUMENTS`:
- Write next failing test
- Go back to Step 3

Each test should be **independent** and test **one thing**.

## Red Flags - STOP and Start Over

If you find yourself:
- Writing code before test
- Test passes immediately
- Can't explain why test failed
- Thinking "I'll test after"
- Saying "too simple to test"
- Saying "keep as reference, write test first"
- Rationalizing "just this once"

**STOP. Delete code. Start over with failing test.**

## Verification Checklist

Before considering work complete:

- [ ] Every new function has a test
- [ ] Watched each test fail before implementing
- [ ] Test failed for the right reason (feature missing)
- [ ] Wrote minimal code to pass
- [ ] All tests pass
- [ ] `gleam check` passes with no warnings
- [ ] No mocks unless absolutely necessary
- [ ] Edge cases covered

Can't check all boxes? You skipped TDD. Start over.

## Gleam Testing Conventions

**Test file location:**
```
src/myapp/user.gleam      → test/myapp/user_test.gleam
```

**Test function naming:**
```gleam
pub fn <behavior>_test() {
  // Test one specific behavior
}
```

**Assertions (use gleeunit/should):**
```gleam
import gleeunit/should

// Value assertions
result |> should.equal(expected)
result |> should.not_equal(wrong)

// Result assertions
result |> should.be_ok
result |> should.be_error

// List assertions
list |> should.equal([1, 2, 3])
```

**Common test patterns:**
```gleam
// Testing error cases
pub fn handles_empty_input_test() {
  process("")
  |> should.be_error
}

// Testing happy path
pub fn creates_valid_user_test() {
  create("email@test.com", "pass123")
  |> should.be_ok
}

// Testing transformation
pub fn formats_name_correctly_test() {
  format_name("john doe")
  |> should.equal("John Doe")
}
```

## Bug Fixes

When fixing a bug:

1. Write failing test that reproduces the bug
2. Watch it fail (confirms test catches the bug)
3. Fix the code
4. Watch test pass
5. All other tests still pass

**Never fix bugs without a test** - you need proof the bug won't return.

## Final Rule

```
Production code → test exists and failed first
Otherwise → not TDD
```

Violating the letter of the rules is violating the spirit of the rules.
