# Truth Serum 💉

> *A Claude Skill that makes Claude incapable of lying to you.*

An institutional-grade working standard for Claude — built for developers who build real, long-term production systems. Not demos. Not experiments. Not temporary code.

> **The goal is not "it works" — it is "it works correctly and reliably one year from now."**

---

## The Idea

The name says it all.

Truth Serum forces Claude to operate like a senior engineer at a top-tier firm:
- If something is broken — it says **broken**
- If something is partial — it says **partial**
- If something is missing — it **stops and asks**, never guesses
- Before touching code — it maps **what depends on what**
- Before committing — it waits for your **explicit approval**

No band-aids. No invented answers. No "it should work."

---

## Core Philosophy

| Principle | Meaning |
|---|---|
| **Truth First** | Never document something as "done" that isn't done |
| **Systematic, Not Point-Fix** | Understand dependencies before fixing anything |
| **Evidence-Based** | Every claim requires `file:line` + quoted code |
| **One Step at a Time** | Checkpoint between every sub-phase, no surprises |

---

## Priority Order

Every decision is evaluated in this sequence:

1. Stability
2. Reliability
3. Readability
4. Maintainability
5. Scalability
6. Performance
7. Elegance

---

## What You Get

| Behavior | Without Skill | With Skill |
|---|---|---|
| Before writing code | Proceeds immediately | Presents assumptions, risks, blast radius |
| When something breaks | May minimize or guess | Reports broken / partial / stubbed accurately |
| When info is missing | Invents or assumes | Stops and asks |
| Committing changes | May commit silently | Requires explicit "commit now" approval |
| Fixing a bug | Fixes the symptom | Maps root cause + all dependent issues |
| Test coverage | "Tests pass" = done | Verifies tests assert real-world behavior |

---

## Installation

### Option A — Claude Code Plugin (recommended)

From within a Claude Code session, two commands and you're done:

```
/plugin marketplace add eliranpv11/claude-code-truth-serum-skill
```

```
/plugin install truth-serum@truth-serum
```

---

### Option B — Claude.ai (Web UI)

1. Download [`truth-serum.skill`](./truth-serum.skill)
2. Go to **claude.ai → Customize → Skills**
3. Click **+** → **Create skill** → Upload the `.skill` file
4. Toggle it on ✅

---

### Option C — Claude Code — Personal Install

Available across **all projects** on your machine:

```bash
mkdir -p ~/.claude/skills

git clone https://github.com/eliranpv11/claude-code-truth-serum-skill.git \
  ~/.claude/skills/truth-serum
```

---

### Option D — Claude Code — Project Install

For a **specific repo only**, shared with your entire team:

```bash
mkdir -p .claude/skills

git clone https://github.com/eliranpv11/claude-code-truth-serum-skill.git \
  .claude/skills/truth-serum

# Commit to share with everyone
git add .claude/skills/
git commit -m "Add Truth Serum working standard"
```

---

### Option E — One-Liner (curl)

```bash
mkdir -p ~/.claude/skills/truth-serum && \
  curl -o ~/.claude/skills/truth-serum/SKILL.md \
  https://raw.githubusercontent.com/eliranpv11/claude-code-truth-serum-skill/main/SKILL.md
```

---

### Option F — Manual Copy

```bash
mkdir -p ~/.claude/skills/truth-serum
cp SKILL.md ~/.claude/skills/truth-serum/SKILL.md
```

---

## Verify Installation

Start a Claude Code session and ask:

```
What working standard are you following?
```

Claude should describe the Truth Serum methodology: truth-first, evidence-based, one-step-at-a-time, no guessing.

---

## Compatibility

- ✅ Claude Code (CLI)
- ✅ Claude.ai (Web & Desktop)
- ✅ Any agent supporting the [Agent Skills open standard](https://code.claude.com/docs/en/skills)

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
