# h00man skills

Agent skills for design and coding work. Each folder in `skills/` is one skill: a `SKILL.md` file with its references and scripts. The skills work in Claude Code, Codex, and other agents that read `SKILL.md` folders.

## Install with `npx skills`

You need Node.js 22.20 or later. The [`skills` CLI](https://github.com/vercel-labs/skills) copies each skill into your agent's skill folder.

Install every skill in the current project:

```sh
npx skills add h00mankind/skills
```

The CLI asks which skills and which agents you want. To install for all your projects, add `-g`:

```sh
npx skills add h00mankind/skills -g
```

To install some skills without the prompts, name the skills and the agents:

```sh
npx skills add h00mankind/skills --skill build --skill ship -a claude-code -a codex -y
```

To see the skills before you install, run `npx skills add h00mankind/skills --list`.

The CLI keeps one copy of each skill in `.agents/skills/` (or `~/.agents/skills/` with `-g`) and links it into the folder of each agent, such as `.claude/skills/`. In a project, it also writes `skills-lock.json`, which pins the version of each skill.

### Install the subagents for Claude Code

In Claude Code, the `build` skill sends work to four subagents in `agents/`. `npx skills` does not install subagents, so copy them yourself:

```sh
git clone --depth 1 https://github.com/h00mankind/skills.git /tmp/h00man-skills
mkdir -p ~/.claude/agents
cp /tmp/h00man-skills/agents/*.md ~/.claude/agents/
```

### Keep `build` next to the skills that read it

`arena`, `council`, `retro`, `ship`, `teach`, and `verification` read files from `build` by relative path, such as `../build/SKILL.md`. When you install one of them, install `build` in the same place.

### Update or remove

```sh
npx skills update
npx skills remove ship
```

## Skills

The agent starts these skills when a request fits:

| Skill | What it does |
|---|---|
| `build` | Picks a playbook for engineering work and runs it: features, bugs, refactors, performance, code review, and "how does X work". |
| `ship` | Commits, pushes, opens the PR, handles review and CI, and merges. |
| `ui` | Designs, builds, or reviews the look of an interface. |
| `motion` | Adds, tunes, or reviews animation and gestures, on the web and in React Native. |
| `verification` | Proves that a change works on the real app, not on a proxy. Also writes a project's verify skill. |
| `writing` | Writes and cleans up docs, READMEs, commit messages, PR descriptions, and agent instructions. |
| `copy` | Writes product and marketing copy: headlines, buttons, errors, and empty states. |
| `present` | Builds a single-file HTML page, deck, diagram, or faux product UI. |
| `prompt` | Writes prompts for image, video, and audio models. |

These skills run only when you type the command, such as `/wdyt`:

| Skill | What it does |
|---|---|
| `wdyt` | Gives an honest verdict on an idea, as a senior peer would. |
| `plan` | Turns a feature into a PRD and vertical-slice issues in `docs/plans/`. |
| `variants` | Builds 3 to 5 different versions of a component or screen behind one picker, then recommends one. |
| `arena` | Gives one task to several agents and compares the results. |
| `council` | Gets a second opinion from several models of different providers. |
| `handoff` | Writes a handoff file so that a fresh agent can continue the work. |
| `retro` | Reviews past agent work and adds a check for each mistake that repeats, so that it does not happen again. |
| `teach` | Explains code or a concept in chat, or runs a course over many sessions. |
| `ambitious` | Turns a rough idea into one detailed prompt that another agent can run. |
| `name` | Generates brand names and screens them for sound and distinctiveness. |
| `bruh` | Says it plainly: your last message without jargon, a picture-first HTML explainer, or very short replies. |

## Requirements

- Node.js, for the scripts in `build`, `retro`, and `present`. Some `build` scripts also need [bun](https://bun.sh).
- The GitHub CLI (`gh`), for `ship`.
- The `codex` CLI, optional. With it, `council`, `arena`, and `ship` can use OpenAI models next to Claude. Without it, they use only the models that your agent can run.

## Choose the model for each role

Skills that start subagents give each role a default model. To change a role, create `~/.claude/agent-models.md`. For Codex, use `${CODEX_HOME:-~/.codex}/agent-models.md`. Each line sets one role:

```text
bug-fix: opus @high
verifiers: codex/gpt-6.1-terra @high | opus @high
```

`@high` sets the effort. `|` separates fallback choices, which the skill tries in order. A role that has no line keeps its default. The Models section of [`skills/build/SKILL.md`](skills/build/SKILL.md) gives the full rules.

## Credits and license

The skills are under the Apache License 2.0. See [`LICENSE`](LICENSE).

Many skills include files from other authors, such as pstack by Lauren Tan, Matt Pocock's skills, and Emil Kowalski's animation skill. Those files keep their own licenses. [`NOTICE.md`](NOTICE.md) lists each source and its license, and each `SKILL.md` ends with a Sources section.
