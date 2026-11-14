---
name: build
description: Route engineering work in code to the right playbook and run it. Use for a feature, a bug with an unknown cause, a refactor, a perf problem, types or architecture ("design this", "architect this"), a code review or audit ("review this", "is this safe to ship", "tear this apart", "what could this break"), "how does X work", "why is X like this", TDD, or a long autonomous coding run. Skip the look of a UI (ui), animation (motion), prose (writing), and PR work and failing CI (ship).
---

# Build

This skill is the entry point for engineering work in code. It takes the place of pstack's `poteto-mode`. It picks a playbook or a reference. The reference holds the steps.

## Before a routed task

1. Read `references/poteto-mode/SKILL.ref.md` in full. It is the rulebook. Its Non-negotiables, Principles, Autonomy, Subagents, Writing the reply, and Comments sections apply to every route below.
2. Where this file and the rulebook differ, follow this file. Follow the user's own instructions (CLAUDE.md, AGENTS.md, the request) over both.
3. Before you apply a principle, read its file, `references/principle-<title>/SKILL.ref.md`.

Skip the playbook for a small task: a contained one-file change with an obvious test, a short question, or a one-line edit. Verify it on the real artifact all the same.

## Routes

Use the first row that fits. Copy the playbook steps into the todo list, as the rulebook says. A bare `.md` name is a playbook in `references/poteto-mode/playbooks/`.

| Request | Read |
|---|---|
| "how does X work", a walkthrough, "where should this live" | `references/how/SKILL.ref.md`, run inside `investigation.md` |
| "why is X like this", history, where a regression came from | `references/why/SKILL.ref.md`, run inside `investigation.md` |
| "are we sure", "X or Y", another read-only question | `investigation.md` |
| a bug with an unknown cause, "debug this" | `bug-fix.md`, with `references/diagnosing-bugs/SKILL.ref.md` (see Choices) |
| a bug with a cheap local test, "write a failing test first" | `references/tdd/SKILL.ref.md` |
| slow code, a perf regression | `perf-issue.md` |
| "make this number better" over many attempts | `hillclimb.md` |
| a live leak, spin, or glitch to diagnose | `runtime-forensics.md` |
| a captured profile, trace, spindump, or heap snapshot | `trace-forensics.md` |
| new or changed behavior | `feature.md` |
| rename, extract, dedupe, or move, with no behavior change | `refactoring.md` |
| types, signatures, or module shape before code | `references/architect/SKILL.ref.md` |
| module depth, seams, testability, "deepen this" | `references/codebase-design/SKILL.ref.md` |
| domain terms, a glossary, an ADR while you design | `references/domain-modeling/SKILL.ref.md` |
| a fork that a run can settle, "prototype it", "sketch it" | `prototype.md` |
| pixel-exact parity between two UIs | `visual-parity.md` |
| review, audit, critique, cleanup, "is this safe to ship" | `references/audit.md` |
| "interrogate", "tear this apart", adversarial review | `references/interrogate/SKILL.ref.md` |
| "what could this break", a brief that asserts a fact about code | `references/blast-radius/SKILL.ref.md` |
| AI slop in a diff | `references/deslop/SKILL.ref.md` |
| comments to cut | `references/no-comments/SKILL.ref.md` |
| parallel coverage, a race, a gauntlet | `references/swarm/SKILL.ref.md` |
| a large or cross-cutting change, or work the user reviews later | `references/figure-it-out/SKILL.ref.md` |
| "run until done", "loop until X" | `autonomous-run.md` |
| a plan across phases or stacked PRs | `multi-phase-plan.md` |
| a queue of PRs to drive with full autonomy | `autopilot-full.md`, or `autopilot-stack.md` when the PRs build on each other |
| a multi-day program for one coordinator chat | `orchestrate.md` |
| resume or take over earlier work | `session-pickup.md` |
| stop cleanly so that work can resume | `pause-safely.md` |
| free disk from worktrees and simulators | `worktree-cleanup.md` |
| a decision trail for long or unattended work | `references/show-me-your-work/SKILL.ref.md` |

## Other h00man skills

- `ship`: commit, branch, PR, babysit, CI, merge conflicts, and landing. When a playbook says "Run Opening a PR", or names the Babysit or Shipping playbook, use `ship`.
- `verification`: a driver or verify skill, formal verification, and the vetting of a benchmark or a measured number.
- `writing`: docs, PR and commit text, and skill authoring (the Authoring a skill playbook).
- `ui`: the look of an interface.
- `motion`: animation and gestures.
The model cannot start a command, so tell the user to type it:

- `/arena` for a bakeoff, a model or skill benchmark, or the Eval playbook.
- `/plan` to grill an idea into a PRD.
- `/variants` for UI variants that the user can switch between.
- `/retro` for a repeated mistake.
- `/ambitious` to sharpen a prompt.
- `/bruh` to restate something in plain words.

## Name map

h00man is standalone. Never load a skill or agent from another source, even when the session has one with the same name. Read each name in the references as follows. A name that is not here is not in the stack: do the step with the files in this skill set, and tell the user the gap so that the set can add it.

- `poteto-mode`: this skill. Its rulebook is `references/poteto-mode/SKILL.ref.md`.
- `how`, `why`, `architect`, `tdd`, `interrogate`, `swarm`, `figure-it-out`, `show-me-your-work`, `blast-radius`, `deslop`, `no-comments`, `diagnosing-bugs`, `codebase-design`, `domain-modeling`, `in-your-own-words`, `ketchup`, `recall`, `teach`, `thermos`, `thermo-nuclear-review`, `thermo-nuclear-code-quality-review`, `typescript-best-practices`: `references/<name>/SKILL.ref.md`. A slash form such as `/how` or `/deslop` is the same file.
- `arena` as a step of another workflow (architect Phase B, interrogate, Feature step 4, blast-radius step 6): follow `references/arena/SKILL.ref.md` yourself. Send a user who asks for a bakeoff to `/arena`.
- A principle skill (`principle-<title>`, or "the <title> principle skill"): `references/principle-<title>/SKILL.ref.md`.
- `GLOSSARY.md` in domain-modeling: the project's existing glossary. If the project has none, use `docs/CONTEXT.md`. `/plan` uses the same file.
- `unslop`, `technical-writing`, `plugin-dev:skill-development`, and `automate-me`, with or without a slash: the `writing` skill. For `automate-me`, write the habit as an h00man skill with the Authoring a skill playbook.
- `babysit`, `fix-ci`, `fix-merge-conflicts`, the Opening a PR, Babysit, and Shipping playbooks, and review-bot triage: the `ship` skill.
- The driver skill, `verify`, `create-verification-skill`, `maintain-verification-skill`, `create-formal-verification-skill`, `maintain-formal-verification-skill`, `statechart`, and `benchmark-checklist` (also the link `../benchmark-checklist/SKILL.md`): the `verification` skill. Claude Code's built-in `run` stays the fallback driver.
- `correct`, `reflect`, and `retro`: `/retro`. `sharpen`: `/ambitious`. `bro` and the link `../bro/SKILL.md`: `../bruh/references/pstack-bro.md`.
- `setup-pstack`: not in the stack. Edit the model sheet by hand (see Models).
- A deploy platform's own skill (Railway, Fly, Vercel, and the like): not in the stack. Read the platform CLI's `--help` and its docs before you run it.
- `loop`, `/loop`, and `run`: Claude Code built-ins.
- The platform maps (`codex-tools.md`, `copilot-tools.md`, `pi-tools.md`, `muse-tools.md`, `t3-tools.md`) name skills of those apps. Read `writing-skills` as the `writing` skill and `pr-stack` as the `ship` skill, on every platform. Use only their tool and model names.

## Paths and agents

- A path such as `skills/poteto-mode/scripts/...` "under the installed plugin" is `skills/build/references/poteto-mode/scripts/...` in this skill set. `<plugin>/skills/reflect/scripts/` is `skills/build/references/reflect/scripts/`.
- Relative links between references, such as `../how/SKILL.md` or `../poteto-mode/references/codex-tools.md`, work as written. The name map covers the three broken links: `../setup-pstack/`, `../benchmark-checklist/`, and `../bro/`.
- Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference links to a `SKILL.md`, read the `SKILL.ref.md` in that folder.
- The scripts need Node.js. The `orch` store and the `watch-pr` watcher also need bun.
- Subagent types: use `build-agent` where a reference says `pstack:poteto-agent`, `comment-sicko` where it says `pstack:comment-sicko`, and the agent of the same name in `agents/` for each pstack thermo agent (`thermo-nuclear-review-subagent`, and the code quality one, `thermo-nuclear-code-quality-review-subagent`). Give each thermo agent the absolute path of its rubric, `references/thermo-nuclear-review/SKILL.ref.md` or `references/thermo-nuclear-code-quality-review/SKILL.ref.md` in this skill.
- Give each `build-agent` and `comment-sicko` the absolute path of this file. The comment-sicko finds the how and why references from it. On every platform, a platform map that gives a child a file in `poteto-mode/references/agents/` or a thermo skill to read first, or a scoped agent id such as `pstack/comment-sicko`, means the agent of that name in `agents/` (`pstack/poteto-agent` is `build-agent`). A Muse skill id `plugin:pstack:<name>` means the file that this map gives for `<name>`: read the `build` skill and read that file, never the pstack id. Its file is `../../agents/comment-sicko.md`, `../../agents/thermo-nuclear-review-subagent.md`, or `../../agents/thermo-nuclear-code-quality-review-subagent.md` from this file.

## Models

- Role models come from the sheet `${CLAUDE_CONFIG_DIR:-~/.claude}/agent-models.md`, as each reference's Models section says. Where an upstream file names `pstack-models.md`, read `agent-models.md`.
- To change a role, edit its line by hand, in the form `bug-fix: opus @high`. A role without a line keeps the reference's default. On Codex, read the sheet `${CODEX_HOME:-~/.codex}/agent-models.md`. The sheet is optional: without it, every role keeps its default.

### Routing

These rules turn a sheet value into a running agent in any harness. They replace the reference's own slug handling where the two differ.

- **Seats and choices.** A comma separates the seats of a panel. A `|` separates the choices for one seat, in order: `codex/gpt-6.1-sol @high | opus @high`. A value without `|` has one choice.
- **Pick the first choice that runs.** Skip a choice when this harness has no route for it (table below). Skip it when the reference rules it out for that seat, such as the arena cross-judge's rule against the parent's model family. Also skip it after it fails: the model is rejected, the account is out of credits, the run errors, or it times out. Then try the next choice.
- **Say so.** When a seat runs on any choice but its first, name the choice that ran and why the first did not.
- **All choices fail.** Drop the seat and say so. If that leaves a single-seat role with no seat, run it on the parent model (`inherit-parent`) and say so, unless the reference rules out the parent's family for that seat. Then follow the reference, for example arena skips the judge.
- **Effort.** `@level` goes to the route's effort field: `effort` on the Claude Code `Agent` call and on a `claudeAgent` delegate, `reasoningEffort` on a `codex` delegate, `-c model_reasoning_effort=<level>` on `codex exec`, and `--effort <level>` on `claude -p`. In T3 Code, pass an option, the fast-mode option included, only when `orchestrator_capabilities` lists it for that model. Some Claude models, such as Fable, have no `fastMode` and reject a call that sends it.

A T3 Code thread runs on Claude Code or on Codex. Use the column of that harness. In T3, a `<provider>/<model>` entry always goes through `delegate_task`.

| Entry | Claude Code (plain or T3) | Codex (plain) | Codex in a T3 thread |
|---|---|---|---|
| `opus`, `sonnet`, `haiku`, `fable` | `Agent` with `model` | `claude -p --model <alias>` | `delegate_task` to `claudeAgent`, with the newest model of that family from `orchestrator_capabilities` |
| `claudeAgent/<id>` | `Agent` with the alias (`claude-fable-5-2` is `fable`); `delegate_task` in T3 | `claude -p --model <id>` | `delegate_task` |
| `codex/<id>` | `codex exec -m <id>`; `delegate_task` in T3 | `spawn_agent` with `model` | `delegate_task` |
| `inherit-parent`, `auto` | subagent with no `model` | subagent with no `model` | subagent with no `model` |

- **CLI routes.** Run a review or other read-only seat as `codex exec -m <id> -s read-only -C <repo> -o <file> "<brief>"`, or `claude -p --model <id> --permission-mode plan "<brief>"`. A seat that writes code gets `-s workspace-write` or the default permission mode, and its own worktree when other seats write too. The brief must stand alone, as for a T3 delegate. Read the result from the output file or stdout. A missing CLI is "no route".

## Context budget

Every turn reads the whole context again from the cache, so each item you keep costs again on each later turn. These rules apply to the main session in every route, and they override the rulebook where the two differ. Other h00man skills point here.

- **Keep images out of the main session.** Send a screenshot, render, or photo comparison to a subagent (`opus @medium`). It returns a short text verdict and the image paths, never the pixels. Read an image yourself only when the user asks to see it.
- **Wait, do not poll.** Act on the completion message of a delegate or background task. Do not loop on `task_status`, `sleep`, or a watcher. When a delegate finishes, read its result, not its whole thread. Session pickup still reads the parts of earlier history that it needs.
- **Cap what a delegate returns.** Tell each delegate to return its verdict, the changed files, and the evidence paths in about 20 lines. Long logs go to a file that the delegate names.
- **Filter output.** Save the full build, test, or log output to a file and keep the command's exit status, for example `cmd > out.log 2>&1; echo "exit $?"`. Then show only the part you need with `tail`, `grep`, or `head`. A pipe such as `cmd | tail` hides a failing exit status. Read a range of a large file, not the whole file.
- **Orchestrate on a cheaper model.** To test a model, run that model as the workers and keep the main session on `opus @medium`. Use the strongest model as the main session only for open-ended creative work.
- **Split a long run into phases.** When a run will compact more than once, end each phase with a handoff file: what is done, the evidence, and the next steps. Continue from that file. Start the next phase in a fresh thread only when the user allows new threads; otherwise continue in the current thread and reread the file after each compaction. A handoff file keeps detail that a compaction summary loses.

## Choices

- **Bug fix.** Do bug-fix steps 1 and 2 with diagnosing-bugs Phases 1 to 4: build a repeatable check that fails on the bug, minimize it, rank hypotheses, tag the probes. Then do bug-fix steps 3 to 6. Run the diagnosing-bugs Phase 6 checklist before the commit. Reproduce the bug yourself before you ask the user for anything. Show the ranked hypotheses, but do not wait for an answer.
- **Review.** Pick by the question. Use `references/audit.md` for a broad sweep with ranked findings and optional fixes. Use interrogate when each finding needs a run as proof. Use blast-radius for effects outside the diff. A deep audit of a risky diff adds a blast-radius pass on its riskiest change.
- **Design.** Follow architect's flow. Compare its candidates with the codebase-design terms (`depth`, `seam`, `adapter`, `leverage`, `locality`). When the design adds or renames a domain term, update the glossary per domain-modeling.
- **Hand-off.** When the work ships a change, finish with `ship`.

## Sources

- **pstack**, by Lauren Tan (poteto), ported to Claude Code by Michael Denyer. Repository: https://github.com/h00mankind/pstack (upstream https://github.com/cursor/plugins/tree/main/pstack). License: MIT. This skill takes the `poteto-mode` rulebook, its playbooks, references, and scripts; the `how`, `why`, `architect`, `tdd`, `interrogate`, `arena`, `swarm`, `figure-it-out`, `show-me-your-work`, `blast-radius`, `no-comments`, `in-your-own-words`, `ketchup`, `recall`, `teach`, `thermos`, `thermo-nuclear-review`, `thermo-nuclear-code-quality-review`, and `typescript-best-practices` skills; the `reflect` scripts; and the 24 principle skills. The hook's trigger rules, `build-agent`, `comment-sicko`, and the two thermo-nuclear agents adapt pstack's SessionStart context and its `poteto-agent`, `comment-sicko`, and thermo-nuclear agents.
- **pstack** `deslop`, from Cursor's cursor-team-kit (https://github.com/cursor/plugins), MIT, copyright Cursor: the AI-slop cleanup pass.
- **matt**, by Matt Pocock. Repository: https://github.com/mattpocock/skills. License: MIT. This skill takes `diagnosing-bugs` (the feedback-loop bug method), `codebase-design` (deep-module vocabulary), and `domain-modeling` (glossary and ADR discipline).
- **h00man:improve**, h00man23's earlier skill, https://github.com/h00mankind/skills, Apache-2.0: the audit areas, modes, checklists, and report format in `references/audit.md`.
