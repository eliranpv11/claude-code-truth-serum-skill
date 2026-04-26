# Truth Serum

> *A Claude skill that makes Claude incapable of lying to you.*

A communication discipline for Claude Code — built for developers who need Claude to report reality accurately, not what it hopes is true.

> **If something is broken — it says broken. If it doesn't know — it stops and asks.**

---

## The Problem

Left unconstrained, Claude Code distorts its own status in predictable ways:

- **Aspirational reporting** — says "done" when something is partial, incomplete, or untested
- **Confidence without evidence** — claims "it should work" without showing any proof
- **Hidden bad news** — minimizes or buries problems rather than surfacing them clearly
- **Silent guessing** — fills in missing information independently, then runs with the wrong interpretation
- **Invisible scope creep** — silently expands the task without flagging the new boundary
- **Phantom test coverage** — reports "tests pass" without showing the actual test output

Truth Serum eliminates all of these failure modes.

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

In a Claude Code session, run:

```
/plugin marketplace add eliranpv11/claude-code-truth-serum-skill
/plugin install truth-serum@truth-serum
```

The skill is installed globally across all your Claude Code sessions immediately.

---

### Option B — Claude.ai (Web & Desktop)

1. Download [`truth-serum.skill`](./truth-serum.skill) from this repository
2. Go to **claude.ai → Customize → Skills**
3. Click **+** → **Create skill** → Upload the `truth-serum.skill` file
4. Toggle it on ✅

The skill is now active for all conversations on Claude.ai.

---

### Option C — Personal install (all your projects)

Install once, available across all local Claude Code projects:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/eliranpv11/claude-code-truth-serum-skill.git \
  ~/.claude/skills/truth-serum
```

---

### Option D — Project install (shared with team)

Install in a project repo so the whole team uses it:

```bash
mkdir -p .claude/skills
git clone https://github.com/eliranpv11/claude-code-truth-serum-skill.git \
  .claude/skills/truth-serum
git add .claude/skills/
git commit -m "chore: add Truth Serum skill"
```

---

### Option E — One-liner (no git required)

Download just the skill file directly:

```bash
mkdir -p ~/.claude/skills/truth-serum && \
  curl -o ~/.claude/skills/truth-serum/SKILL.md \
  https://raw.githubusercontent.com/eliranpv11/claude-code-truth-serum-skill/main/SKILL.md
```

---

### Option F — CLAUDE.md paste (zero-install, works everywhere)

The `CLAUDE.md` file contains the complete skill content without YAML frontmatter — ready to paste directly into any project.

1. Copy the contents of [`CLAUDE.md`](./CLAUDE.md)
2. Paste into your project's `CLAUDE.md` file (merge with existing content if needed)

This approach requires no installation and works in any environment that reads `CLAUDE.md`.

---

### Option G — Cursor IDE

1. Copy [`.cursor/rules/truth-serum.mdc`](./.cursor/rules/truth-serum.mdc) to your project's `.cursor/rules/` directory
2. Cursor will apply the rule automatically on every request (`alwaysApply: true`)

Or install manually:

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/truth-serum.mdc \
  https://raw.githubusercontent.com/eliranpv11/claude-code-truth-serum-skill/main/.cursor/rules/truth-serum.mdc
```

---

## How to Use

Once installed, Truth Serum applies automatically. Claude will self-enforce honest reporting, evidence standards, and the commit gate without any prompting.

You can also invoke it explicitly:

> "Apply truth-serum standards to this session."
> "Report status per truth-serum."

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
| **[code-discipline](https://github.com/eliranpv11/code-discipline-skills)** | Coding methodology | You need Claude to write code the right way |
| **[autonomous-agent-protocol](https://github.com/eliranpv11/autonomous-agent-protocol-skill)** | Execution framework | Claude runs without mid-task human approvals |

Load all three for the complete stack. Load any one standalone — each works independently.

---

## Compatibility

- ✅ Claude Code (CLI)
- ✅ Claude.ai (Web & Desktop)
- ✅ Cursor IDE
- ✅ Any agent supporting the Agent Skills open standard

---

## File Structure

```
claude-code-truth-serum-skill/
├── SKILL.md                        ← Install as a Claude Code skill
├── CLAUDE.md                       ← Copy into any project's CLAUDE.md
├── README.md                       ← This file
├── EXAMPLES.md                     ← Real before/after examples
├── LICENSE                         ← MIT
├── truth-serum.skill               ← Packaged for claude.ai upload
├── .claude-plugin/
│   ├── plugin.json                 ← Plugin metadata
│   └── marketplace.json            ← Marketplace listing
└── .cursor/
    └── rules/
        └── truth-serum.mdc         ← Cursor IDE rules (alwaysApply: true)
```

---

## License

MIT — use it, fork it, adapt it to your own standard.
