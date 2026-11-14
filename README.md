# h00man skills

Agent skills for design and coding work, by h00man23. They work in Claude Code and Codex, and in other harnesses that read `SKILL.md` folders.

Each folder in `skills/` is one standalone skill. Copy the skills you want. You do not need a plugin or a marketplace.

## Skills

The model starts these skills on its own when a request fits:

| Skill | What it does |
|---|---|
| `build` | Routes engineering work (features, bugs, refactors, perf, reviews, "how does X work") to the right playbook and runs it. |
| `ship` | Commits, pushes, opens the PR, watches review and CI, and merges. |
| `ui` | Designs, builds, or reviews the look of an interface. |
| `motion` | Adds, tunes, or reviews animation and gestures, on the web and in React Native. |
| `verification` | Proves that behavior works on the real artifact, not a proxy. |
| `writing` | Writes or cleans up docs, READMEs, commit messages, and agent instructions. |
| `copy` | Writes product and marketing copy. |
| `present` | Builds a single-file HTML page, deck, diagram, or faux product UI. |
| `prompt` | Writes prompts for image, video, and audio models. |

These skills run only when you type them, for example `/wdyt`:

| Skill | What it does |
|---|---|
| `wdyt` | Gives an honest verdict on an idea, as a senior peer would. |
| `plan` | Turns a feature into a PRD with vertical-slice issues. |
| `variants` | Builds 3 to 5 different takes on a component or screen behind one picker. |
| `arena` | Puts several agents on one task and compares the results. |
| `council` | Gets a second opinion from several models of different providers. |
| `handoff` | Writes a handoff file so a fresh agent can continue the work. |
| `retro` | Looks back on agent work and stops its mistakes from coming back. |
| `teach` | Explains code or a concept, or runs a course over many sessions. |
| `ambitious` | Turns a rough idea into one ambitious prompt for another agent. |
| `name` | Generates and screens brand names. |
| `bruh` | Says it plainly, in simple words or very short. |

## Install

Clone the repository:

```sh
git clone https://github.com/h00mankind/skills.git
```

**Claude Code.** Copy skills to `~/.claude/skills/` for all projects, or to `.claude/skills/` in one project. The `build` skill also uses the subagents in `agents/`. Copy them to `~/.claude/agents/`.

```sh
cp -R skills/skills/* ~/.claude/skills/
cp skills/agents/*.md ~/.claude/agents/
```

**Codex.** Copy skills to `~/.agents/skills/` or `${CODEX_HOME:-~/.codex}/skills/`.

Some skills read others by relative path, for example `../build/SKILL.md`. Keep `build` installed when you use `arena`, `council`, `retro`, `ship`, `teach`, or `verification`.

## Requirements

- Node.js for the scripts in `build`, `retro`, and `present`. Some `build` scripts also need bun.
- The GitHub CLI (`gh`) for `ship`.
- The `codex` CLI is optional. Without it, `council` and `ship` use the models that your harness has.

## Model choice

Skills that start subagents name a default model for each role. To change a role, write an optional sheet at `~/.claude/agent-models.md` (Codex: `${CODEX_HOME:-~/.codex}/agent-models.md`). Each line is one role:

```text
bug-fix: opus @high
verifiers: codex/gpt-6.1-terra @high | opus @high
```

A `|` gives fallback choices, tried in order. A role without a line keeps its default. The Models section of `skills/build/SKILL.md` has the full rules.

## Credits

Many skills build on work by other authors. `NOTICE.md` lists each source and its license.

## License

Apache-2.0. See `LICENSE`. Third-party parts keep their own licenses, as `NOTICE.md` says.
