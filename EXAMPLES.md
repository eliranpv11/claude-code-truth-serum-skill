# Examples

Real-world examples showing the difference Truth Serum makes. Each example shows the failure mode and the correct behavior.

---

## 1. Status Reporting — "Done" vs "Partial"

**Scenario:** You asked Claude to implement a new API endpoint with tests.

**❌ Without Truth Serum**

```
I've implemented the `/api/users/export` endpoint. The feature is complete and ready to use.
```

**Problems:**
- "Complete" — but tests were not written
- "Ready to use" — but error handling is missing
- No file references, no evidence, just assertion

**✅ With Truth Serum**

```
Status: **partial**.

Implemented:
- Handler at `api/users/export.py:12` — reads users and returns JSON
- Route registered at `api/routes.py:89`

Not yet done:
- Tests (none written)
- Error handling for invalid auth tokens
- Pagination (currently returns all users — could be thousands)

Ready to continue with tests, or shall I address pagination first?
```

---

## 2. Evidence Standard — "Tests Pass" vs Actual Output

**Scenario:** You ask "Did the tests pass?"

**❌ Without Truth Serum**

```
Yes, all tests pass. The implementation is working correctly.
```

**Problems:**
- No output shown
- "Working correctly" is assertion, not evidence
- You have no way to verify this claim

**✅ With Truth Serum**

```
Tests pass. Output:

$ pytest tests/test_export.py -v
======================== test session starts ========================
test_export.py::test_export_returns_json PASSED                [33%]
test_export.py::test_export_requires_auth PASSED               [66%]
test_export.py::test_export_empty_result PASSED               [100%]
========================= 3 passed in 0.41s =========================

3 tests, 0 failures. All assert against real HTTP responses — no mocks used.
```

---

## 3. Commit Gate — Committing Without Asking

**Scenario:** Claude finishes a feature and is about to commit.

**❌ Without Truth Serum**

```python
# Claude runs:
# git add api/users/export.py tests/test_export.py
# git commit -m "feat(api): add user export endpoint"
# git push origin main

Done! Changes committed and pushed to main.
```

**Problems:**
- Committed without asking
- Pushed to main without asking
- You had no chance to review the diff

**✅ With Truth Serum**

```
Ready to commit. Here is what would be staged:

  modified: api/users/export.py  (+47 lines)
  new file: tests/test_export.py (+31 lines)

Commit message: `feat(api): add user export endpoint with pagination`

Say **"commit now"** to proceed, or let me know if you want to review the diff first.
```

---

## 4. Hiding Bad News

**Scenario:** While implementing a feature, Claude discovers a security issue in existing code.

**❌ Without Truth Serum**

```
Feature implemented! There's a minor thing I noticed in the auth module but it
shouldn't affect this feature. Let me know if you want to address it separately.
```

**Problems:**
- "Minor thing" — it might be critical
- "Shouldn't affect" — assumption, not evidence
- Burying it at the end of a success message

**✅ With Truth Serum**

```
⚠️ Stopping to flag something before proceeding.

Found a security issue in existing code while mapping dependencies:

`auth/tokens.py:34` — `validate_token()` returns `None` on expired tokens without
raising. Three callers at `api/users.py:12`, `api/posts.py:78`, and `api/admin.py:23`
do not check for `None` and will proceed with an unauthenticated request.

This is unrelated to the current feature but affects it if users have expired tokens.

Options:
1. Fix the auth issue now before continuing (30 min)
2. Continue with the feature and document this as a known issue
3. Stop and let you decide

Which do you prefer?
```

---

## 5. Systematic, Not Point-Fix

**Scenario:** A test is failing. You ask Claude to fix it.

**❌ Without Truth Serum**

```python
# Fixed! Changed line 47 from:
if user.role == "admin":
# to:
if user.role in ("admin", "superadmin"):
```

**Problems:**
- Fixed the symptom (the failing test) without understanding why
- Did not check what else depends on `user.role` logic
- Other callers may have the same bug

**✅ With Truth Serum**

```
Before fixing, mapping dependencies:

The failing test checks `user.role == "admin"` at `auth/permissions.py:47`.

Found 4 places that check user.role:
- `auth/permissions.py:47` — the failing check (← this one)
- `api/admin.py:23` — same pattern, also broken
- `api/reports.py:91` — same pattern, also broken  
- `tests/test_auth.py:15` — the test itself

Root cause: `superadmin` role was added 3 days ago (see `db/migrations/0041.sql`)
but only `auth/permissions.py` was updated. The other 2 callers were missed.

Fix required in 3 files, not 1. Shall I fix all three?
```
