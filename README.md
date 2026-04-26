# Truth Serum

> *A Claude skill that makes Claude incapable of lying to you.*

A communication discipline for Claude Code — built for developers who need Claude to report reality accurately, not what it hopes is true.

> **If something is broken — it says broken. If it doesn't know — it stops and asks.**

---

## The Idea

The name says it all.

Truth Serum forces Claude to operate with complete honesty:
- If something is broken — it says **broken**, not "almost there"
- If something is partial — it says **partial**, not "done"
- If information is missing — it **stops and asks**, never guesses
- Before committing — it waits for your **explicit approval**
- Every claim comes with **evidence**: `file:line` + quoted code or actual test output

No invented answers. No "it should work." No aspirational status updates.

---

## What Truth Serum owns

Truth Serum is a **communication and honesty layer**. It controls:

| Area | What it enforces |
|---|---|
| Status reporting | Broken/partial/stubbed — never aspirational |
| Evidence standard | `file:line` + quoted code for every claim |
| Commit gate | Explicit "commit now" / "push now" / "deploy now" required |
| Documentation | Single source of truth, no contradictions |
| Mistake handling | Acknowledge, root-cause, fix systemically |
| Scope discipline | Document discovered issues, don't silently expand |

It does **not** own: how code is written, priority order, or coding methodology — those belong to **code-discipline**.

---

## Before & After

| Without Truth Serum | With Truth Serum |
|---|---|
| Says "done" — but it's partial | Says "partial" — explains what remains |
| Says "tests pass" with no output | Pastes actual test runner output |
| Claims "it should work" | Provides `file:line` + quoted code |
| Commits silently | Waits for explicit "commit now" |
| Hides bad news or minimizes it | Surfaces problems immediately and clearly |
| Guesses when info is missing | Stops and asks |
| Patches the symptom | Maps root cause before touching anything |

---

## Core Philosophy

| Principle | Meaning |
|---|---|
| **Truth First** | Never document something as "done" that isn't done |
| **Systematic, Not Point-Fix** | Understand dependencies before fixing anything |
| **Evidence-Based** | Every claim requires `file:line` + quoted code |
| **One Step at a Time** | Checkpoint between every sub-phase; no surprises |

---

## Installation

### Option A — Claude Code Plugin (recommended)

```
/plugin marketplace add eliranpv11/claude-code-truth-serum-skill
/plugin install truth-serum@truth-serum
```

### Option B — Claude.ai (Web UI)

1. Download [`truth-serum.skill`](./truth-serum.skill)
2. Go to **claude.ai → Customize → Skills**
3. Click **+** → **Create skill** → Upload the `.skill` file
4. Toggle it on ✅

### Option C — Personal install

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/eliranpv11/claude-code-truth-serum-skill.git \
  ~/.claude/skills/truth-serum
```

### Option D — Project install (shared with team)

```bash
mkdir -p .claude/skills
git clone https://github.com/eliranpv11/claude-code-truth-serum-skill.git \
  .claude/skills/truth-serum
git add .claude/skills/
git commit -m "Add Truth Serum skill"
```

### Option E — One-liner

```bash
mkdir -p ~/.claude/skills/truth-serum && \
  curl -o ~/.claude/skills/truth-serum/SKILL.md \
  https://raw.githubusercontent.com/eliranpv11/claude-code-truth-serum-skill/main/SKILL.md
```

---

## Verify Installation

Start a Claude Code session and ask:

```
What working standard are you following?
```

Claude should describe truth-first reporting, evidence requirements, the commit gate, and the one-step-at-a-time principle.

---

## The Skill Suite

Truth Serum is part of a three-skill suite — each owns a distinct layer:

| Skill | Layer | Use when |
|---|---|---|
| **truth-serum** | Honesty & communication | You need Claude to report reality accurately |
| **[code-discipline](https://github.com/eliranpv11/code-discipline)** | Coding methodology | You need Claude to write code the right way |
| **[autonomous-agent-protocol](https://github.com/eliranpv11/autonomous-agent-protocol)** | Execution framework | Claude runs without mid-task human approvals |

Load all three for the complete stack. Load any one standalone — each works independently.

---

## Compatibility

- ✅ Claude Code (CLI)
- ✅ Claude.ai (Web & Desktop)
- ✅ Any agent supporting the Agent Skills open standard

---

## File Structure

```
truth-serum/
├── README.md          ← You are here
├── SKILL.md           ← The skill itself
└── truth-serum.skill  ← Packaged for claude.ai upload
```

---

## License

MIT — use it, fork it, adapt it to your own standard.
