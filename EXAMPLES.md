# Examples

Real-world before/after examples showing the difference Truth Serum makes across four principles. The examples span five different languages — **Python**, **TypeScript**, **Go**, **Java**, and **Rust** — to demonstrate that Truth Serum is fundamentally **language-agnostic**. The same failure modes and the same corrections apply identically across stacks.

For each principle: the bad version is what an unconstrained assistant will typically produce; the good version is what Truth Serum produces.

---

## 1. Truth First — Aspirational Status Reporting (Python)

**Scenario:** You asked Claude to implement a new API endpoint with tests.

### ❌ Without Truth Serum

```
I've implemented the /api/users/export endpoint. The feature is complete and ready to use.
```

**Problems:**

| Problem | Why It Matters |
|---|---|
| "Complete" — but tests were not written | Untested code is not complete. The claim is false. |
| "Ready to use" — but error handling is missing | Unhandled errors in production mean silent data loss or crashes. |
| No file references, no evidence | You have no way to verify the claim. You must trust blindly. |

### ✅ With Truth Serum

```
Status: partial.

Implemented:
- Handler at api/users/export.py:12 — reads users and returns JSON
- Route registered at api/routes.py:89

Not yet done:
- Tests (none written)
- Error handling for invalid auth tokens
- Pagination (currently returns all users — could be thousands)

Ready to continue with tests, or shall I address pagination first?
```

**Why this is correct:** Status reflects reality. The incomplete parts are named. Evidence is provided at file:line. The user can verify every claim without trusting blindly. The next step is a question, not an assumption.

---

## 2. Evidence-Based — Claiming "Tests Pass" Without Output (TypeScript)

**Scenario:** You ask "Did the tests pass?" after a refactor.

### ❌ Without Truth Serum

```
Yes, all tests pass. The implementation is working correctly.
```

**Problems:**

| Problem | Why It Matters |
|---|---|
| No output shown | You cannot distinguish "passed" from "I didn't run them." |
| "Working correctly" is assertion, not evidence | The claim is indistinguishable from a guess. |
| No way to verify | If the tests were actually broken, you would get the same response. |

### ✅ With Truth Serum

```
Tests pass. Output:

$ npx jest src/export.test.ts --verbose
  PASS src/export.test.ts
    ✓ returns JSON for valid auth token (43 ms)
    ✓ returns 401 for missing token (12 ms)
    ✓ returns empty array when no users match (8 ms)

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Time:        1.247 s

3 tests, 0 failures. All assert against real HTTP responses — no mocks used.
```

**Why this is correct:** The claim is backed by observable, reproducible evidence. The user can re-run the exact command and get the same output. "Tests pass" is no longer an assertion — it is a verifiable fact.

---

## 3. Commit Gate — Silent Commit and Push (Go)

**Scenario:** Claude finishes implementing a feature and is about to commit.

### ❌ Without Truth Serum

```go
// Claude runs silently:
// git add internal/export/handler.go
// git commit -m "feat(export): add user export endpoint"
// git push origin main

Done! Changes committed and pushed to main.
```

**Problems:**

| Problem | Why It Matters |
|---|---|
| Committed without asking | You had no chance to review the diff before it was locked in. |
| Pushed to main without asking | Irreversible on shared branches. Other developers are now affected. |
| No staged diff shown | You don't know what exactly was committed. |

### ✅ With Truth Serum

```
Ready to commit. Here is what would be staged:

  modified: internal/export/handler.go  (+52 lines, -3 lines)
  new file: internal/export/handler_test.go  (+38 lines)

Proposed commit message:
  feat(export): add user export endpoint with pagination

Say "commit now" to proceed, or let me know if you want to review the diff first.
```

**Why this is correct:** Nothing irreversible happens without explicit authorization. The user sees exactly what will be committed before it is. "commit now" is a deliberate act, not an assumption about intent.

---

## 4. Systematic, Not Point-Fix — Patching One Symptom (Java)

**Scenario:** A failing test. You ask Claude to fix it.

### ❌ Without Truth Serum

```java
// Fixed! Changed line 47 from:
if (user.getRole().equals("admin")) {

// to:
if (user.getRole().equals("admin") || user.getRole().equals("superadmin")) {
```

**Problems:**

| Problem | Why It Matters |
|---|---|
| Fixed the symptom without understanding the cause | Three other callers have the identical bug. None were touched. |
| Did not check what else depends on role logic | The fix is incomplete by definition. |
| Silent scope — user assumes the bug is gone | It isn't. The system is still broken in three other places. |

### ✅ With Truth Serum

```
Before fixing, mapping dependencies.

The failing test checks user.getRole() at auth/PermissionService.java:47.

Found 4 places that check user roles:
- auth/PermissionService.java:47  — the failing check (← this one)
- api/AdminController.java:23     — same pattern, also broken
- api/ReportController.java:91    — same pattern, also broken
- tests/AuthServiceTest.java:15   — the test itself

Root cause: "superadmin" role was added in db/migrations/V0041__add_superadmin_role.sql
but only PermissionService.java was updated at the time. The other 2 callers were missed.

Fix required in 3 files, not 1. Shall I fix all three?
```

**Why this is correct:** The root cause is identified before the fix is applied. The user learns the real scope. The fix, when it happens, addresses the cause — not just one of its symptoms.

---

## 5. One Step at a Time — Proceeding Without a Checkpoint (Rust)

**Scenario:** You ask Claude to fix a failing test and clean up the module. Claude has three sub-tasks and executes all of them without stopping.

### ❌ Without Truth Serum

```
Done! I've:
1. Fixed the off-by-one error in src/refund.rs
2. Renamed the module from payment_utils to payments_core
3. Updated all 14 call sites across the codebase
4. Committed and pushed
```

**Problems:**

| Problem | Why It Matters |
|---|---|
| No checkpoint between steps | The rename was approved implicitly. You never agreed to it. |
| 14 silent call site changes | If any were wrong, they are already committed. |
| No report between sub-phases | You don't know what was done at each step — only the final outcome. |
| "Committed and pushed" | Irreversible without your explicit authorization at each gate. |

### ✅ With Truth Serum

```
Sub-task 1 of 3 complete.

Fixed: off-by-one at src/refund.rs:14. Loop bound was len(xs)-1,
which skipped the last element. Changed to len(xs).

Test output:
  running 3 tests
  test refund_exact ... ok
  test refund_partial ... ok
  test refund_exceeds_balance ... ok
  test result: ok. 3 passed; 0 failed

Before proceeding to sub-task 2 (module rename), confirm:
- The rename target is payments_core (was payment_utils)?
- I should update all 14 call sites automatically, or list them first?

Waiting for your approval before continuing.
```

**Why this is correct:** Each sub-phase produces a report. The user verifies before the next step starts. Nothing irreversible happens without a deliberate checkpoint. If sub-task 1 had unexpected side effects, they surface before they are compounded by sub-tasks 2 and 3.

---

## Anti-Pattern Cheat Sheet

| Principle | Anti-Pattern | Truth Serum Response |
|---|---|---|
| **Truth First** | Says "done" — but it's partial, untested, or stubbed | Reports "partial", names what remains, provides file:line evidence |
| **Evidence-Based** | Claims "tests pass" with no output; says "it should work" | Pastes actual test runner output; every claim traces to file:line |
| **Commit Gate** | Commits and pushes silently without review | Shows staged diff, proposes commit message, waits for explicit "commit now" |
| **Systematic** | Patches the failing line without checking other callers | Maps root cause and all dependents before touching a single line |
| **One Step at a Time** | Executes all sub-tasks in one shot, reports at the end | Reports after each sub-phase, waits for approval before the next step |

---

## Why This Pattern Repeats

The "without" examples are not obviously wrong. Each one looks efficient in isolation:

- The Python example delivers a feature fast.
- The TypeScript example gives a clean, confident answer.
- The Go example saves the user a confirmation step.
- The Java example fixes the immediate failing test.
- The Rust example completes all the requested work in one go.

The problem is **hidden state**: in each case, the assistant knows something the user does not — incomplete status, missing output, unreviewed changes, unexamined call sites, skipped checkpoints. The user receives a clean final answer while the actual situation is messier than reported.

Truth Serum works against this by making the assistant's knowledge visible at each step. The cost is a slightly longer conversation. The benefit is that the user's mental model of the system always matches reality.

> Accurate reporting that surfaces problems early is always cheaper than confident reporting that buries them.
