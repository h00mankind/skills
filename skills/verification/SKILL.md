---
name: verification
description: Prove that behavior works on the real artifact, not a proxy. Use to make or update a project verify or driver skill, also after the app changed ("make a verify skill", "audit the verify skill", "the driver skill is out of date"), to formally verify a race, retry, or invariant ("model-check this", "prove this invariant"), to vet a benchmark or a measured speedup or regression, or to check work before you call it done.
---

# verification

Prove each claim on the real thing. Leave the project a scripted way to prove it again.

## Pick the job

Match the request to one job. Read the named file in full before you start, and follow it.

- **Done check.** You finished a task and want to say so. Read `references/pstack-prove-it-works.md`. Take from it what counts as proof and how to script the check.
- **Make a driver skill.** The project has no scripted way to launch the app, drive it like a user, and capture evidence. Read `references/create-verification-skill/SKILL.ref.md`. Take the repo interview, the sections of the generated `verify` skill, and the proof step. Use the example map in `references/create-verification-skill/references/feature-map-example/` as the pattern for the feature files.
- **Keep a driver skill current.** The app changed, or the user wants an audit of the project's `verify` skill. Read `references/pstack-maintain-verification-skill.md`. Take its outcomes, its edit scope, and the pass.
- **Make a formal check.** Concurrency, retries, a state machine, a parser, or unsafe code needs more than tests. Read `references/pstack-create-formal-verification-skill.md`. Take how to choose the target and the tool, the quality bar, and the hand-off.
- **Keep a formal check current.** Code under a formal check changed without a recheck. Read `references/pstack-maintain-formal-verification-skill.md`. Take its outcomes and the pass.
- **Vet a number.** You measured a speedup, a regression, or a choice between options. Read `references/pstack-benchmark-checklist.md`. Answer its seven questions before you report or act on the number.

A task can need two jobs. A perf fix needs the benchmark checklist and then the done check.

## Order of work

1. Find what the project already has. Look for a project skill named `verify` or `verify-*`, formal check skills, benchmark scripts, and test harnesses. Extend what exists before you write anything new.
2. Fix a broken build or launch first. A check that you write against a broken base records wrong steps.
3. Run every check you write at least once. Show that it can fail. A check that never ran is a draft.
4. Keep the evidence where the next reader can find it. Say where it is in your report.
5. Report what you proved, what you did not prove, and why.

## h00man choices

- Decide from the repo. Do not ask the user what you can observe by running something.
- Prefer the real user path to a mock. Use a mock only at a boundary that production already isolates.
- Write generated skills for an agent that reads them mid-task with no other context. Use the repo's real commands, paths, and names.
- A generated driver skill or formal check skill is project code. Ship it through the `ship` skill, in a change of its own.
- When a check fails on current code, report the finding. Do not weaken the check to make it pass.
- Follow the Context budget section of the `build` skill. A visual check runs in a subagent that returns a text verdict and the screenshot paths, not the images.

## Names in the included files

The included files come from pstack and name other pstack parts. h00man is standalone, so never load a pstack skill even when the session has one. Map each name like this:

- The "platform mapping" lines at the top of each file point to `poteto-mode`, the rulebook of the `build` skill. Skip them. On Codex or another harness, use that harness's own tools for subagents and questions.
- `/create-verification-skill` and `/maintain-verification-skill` mean the two driver-skill jobs above in this skill.
- `/create-formal-verification-skill` and `/maintain-formal-verification-skill` mean the two formal jobs above in this skill.
- `/verify` is the project skill that the driver job writes. It is not part of h00man.
- `/statechart` and the link `../statechart/SKILL.md`: `references/statechart/SKILL.ref.md`. Its runner is `references/statechart/scripts/statechart`. The generated skill names the h00man `verification` skill and this reference, not a path, because paths change between machines. It never names `/statechart` or pstack.
- The **show-me-your-work** skill: `../build/references/show-me-your-work/SKILL.ref.md` when `build` is installed. Without it, keep the check script and its output in the PR or a linked notes file.
- The **Opening a PR** playbook means the `ship` skill.
- The **Perf issue** and **Hillclimb** playbooks: `../build/references/poteto-mode/playbooks/perf-issue.md` and `hillclimb.md` there. Run them through the `build` skill, and use this skill to vet each number they produce.

Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference links to a `SKILL.md`, read the `SKILL.ref.md` in that folder.

## Sources

- pstack, by Lauren Tan, ported for Claude Code and Codex by h00man (https://github.com/cursor/plugins, folder `pstack`; local port in `sources.toml`), MIT. Takes `create-verification-skill` with its feature-map example, `maintain-verification-skill`, `create-formal-verification-skill`, `maintain-formal-verification-skill`, `statechart` (without its tests), `benchmark-checklist`, and `principle-prove-it-works`.
