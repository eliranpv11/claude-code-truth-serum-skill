# Using Truth Serum with Codex

This repository ships with a Codex-compatible skill so the Truth Serum discipline can be invoked directly from the Codex CLI. This document covers installation, invocation, configuration, and what to keep in sync when contributing.

---

## How It Works in Codex

The Codex CLI looks for skills under `~/.codex/skills`. Each skill is a folder that contains:

1. A `SKILL.md` file with the methodology text and YAML frontmatter (already present in this repo at [`skills/truth-serum/SKILL.md`](skills/truth-serum/SKILL.md)).
2. A Codex-specific metadata file at `agents/openai.yaml` ([`skills/truth-serum/agents/openai.yaml`](skills/truth-serum/agents/openai.yaml)) that controls UI presentation, the default prompt, and invocation policy.

When the skill folder is installed under `~/.codex/skills/truth-serum`, Codex makes it available across every project.

---

## Installation

From the root of this repository, copy or symlink the skill folder into the Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R skills/truth-serum ~/.codex/skills/
```

Or, to keep the installed copy in sync with `git pull` on this repository, symlink instead:

```bash
mkdir -p ~/.codex/skills
ln -s "$(pwd)/skills/truth-serum" ~/.codex/skills/truth-serum
```

After installing, restart Codex so it picks up the new skill.

---

## Invocation

### Explicit Invocation

Once installed, the skill can be invoked by name in any Codex prompt:

```
$truth-serum
```

When you include `$truth-serum` in a prompt, Codex loads the methodology and applies its principles to whatever task follows. You can combine it with the actual request:

```
$truth-serum report status on the current feature branch
```

### Implicit Invocation

The bundled `openai.yaml` sets `policy.allow_implicit_invocation: true`. This means Codex may activate the skill automatically when the task description matches the skill's trigger phrases (defined in the `description` field of `SKILL.md`).

If you want manual-only invocation, edit your local copy of `agents/openai.yaml` and set `allow_implicit_invocation: false`. The skill will then only activate when you explicitly type `$truth-serum`.

---

## The `openai.yaml` File

This is the structure Codex reads:

```yaml
interface:
  display_name: "Truth Serum"
  short_description: "<one-line summary shown in the Codex UI>"
  default_prompt: "<prompt Codex injects when the skill is invoked>"
policy:
  allow_implicit_invocation: true
```

| Field | Purpose |
|---|---|
| `interface.display_name` | Human-readable name shown in the Codex skill list. |
| `interface.short_description` | One-line summary shown in the UI. Keep it short and action-oriented. |
| `interface.default_prompt` | The text Codex prepends when the skill is invoked. This is what shapes the assistant's behavior. |
| `policy.allow_implicit_invocation` | If `true`, Codex can activate the skill automatically when the task fits. If `false`, only explicit `$truth-serum` invocation works. |

The version bundled in this repository sets implicit invocation **on** by default — the honesty discipline is generally useful and rarely intrusive. Switch it to `false` if you prefer to stay in explicit-only mode.

---

## Codex vs. Claude Code vs. Cursor

| Surface | Use this skill format |
|---|---|
| **Codex CLI** | The full skill folder installed under `~/.codex/skills/truth-serum` (includes `SKILL.md` + `agents/openai.yaml`) |
| **Claude Code (CLI)** | The plugin installed via `/plugin install` |
| **Claude.ai (Web/Desktop)** | The skill content pasted into a custom skill |
| **Cursor IDE** | `.cursor/rules/truth-serum.mdc` (committed in repo or copied) |
| **Any project, any tool** | The contents of `CLAUDE.md` pasted into your project root |

Each surface reads a different file format — none of them interfere with the others. You can install the skill on multiple surfaces in the same project; they will not conflict.

---

## Troubleshooting

### Codex does not list the skill

- Confirm the folder is installed at exactly `~/.codex/skills/truth-serum`. The folder name must match `truth-serum`.
- Confirm `~/.codex/skills/truth-serum/SKILL.md` exists.
- Confirm `~/.codex/skills/truth-serum/agents/openai.yaml` exists and is valid YAML.
- Restart Codex. The skill list is read at startup.

### Codex lists the skill but `$truth-serum` does nothing

- Open `~/.codex/skills/truth-serum/agents/openai.yaml` and confirm the `interface.default_prompt` field is non-empty.
- Confirm there are no YAML syntax errors — a missing colon or unclosed quote will silently disable the skill.

### Codex never invokes the skill implicitly

- This is controlled by `policy.allow_implicit_invocation`. Set it to `true` if you want automatic activation.
- The trigger phrases live in `SKILL.md` under the `description` field. If the description does not include phrases that match your task wording, Codex will not auto-invoke. Either expand the description or invoke explicitly with `$truth-serum`.

### Codex applies the skill on tasks where it shouldn't

- Set `policy.allow_implicit_invocation: false` to limit activation to explicit invocation only.
- Or narrow the `description` field in `SKILL.md` to remove overly broad trigger phrases.

---

## For Contributors

If you change the four philosophies or any other content shared across surfaces, update **all** of the following so they stay in sync:

| File | Purpose |
|---|---|
| [`CLAUDE.md`](CLAUDE.md) | Zero-install, paste-into-any-project version |
| [`.cursor/rules/truth-serum.mdc`](.cursor/rules/truth-serum.mdc) | Cursor IDE rule |
| [`skills/truth-serum/SKILL.md`](skills/truth-serum/SKILL.md) | Claude Code plugin / Claude.ai / Codex skill body |
| [`skills/truth-serum/agents/openai.yaml`](skills/truth-serum/agents/openai.yaml) | Codex-specific UI metadata and invocation policy |

The methodology text in the first three files must be identical. The `openai.yaml` does not duplicate the methodology — it only carries Codex-specific fields — but the `default_prompt` should reflect the principles accurately. If you add or remove a principle, update the prompt accordingly.

The version numbers in `.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json`, and `skills/truth-serum/SKILL.md` (`metadata.version`) must all match. Bump them together.
