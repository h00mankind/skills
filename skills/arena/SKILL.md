---
name: arena
description: Put several agents on one task. The default arena mode runs parallel candidates and grafts the best parts into one result, and benchmark ranks models or skills with a blind judge. Runs only when the user types /arena.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# arena

Use several agents where one attempt or one opinion is not enough.

## Pick the mode

The first word of the argument picks the mode. The rest of the argument is the task.

- **arena** (the default): several candidates make the same artifact in parallel. You pick a base and graft the best parts of the others into it. Use it to make one thing when a single attempt could commit to the wrong design.
- **benchmark**: contenders do the same task, a blind judge scores them, and an HTML report ranks them. Use it to compare models or skills. It does not make a deliverable.

With no mode word, choose from the request. A comparison of models or skills runs benchmark. Anything else runs arena. Name the mode in your first line.

A review or a second opinion on existing work is not an arena task. Tell the user to type `/council`.

## Models and harness

These rules apply in every mode.

- Read role lines from `agent-models.md` in `${CLAUDE_CONFIG_DIR:-$HOME/.claude}/` when that file exists. Without it, use the defaults in the mode's file.
- Route every seat with the Routing rules in the Models section of `../build/SKILL.md`: the `|` fallback choices, the route for each harness, and the effort field. In T3 Code, load `delegate_task` and `orchestrator_capabilities` first if they are deferred tools.
- Set each agent's reasoning effort for its job. Go higher for hard design or review work. Go lower for work that is mostly generation.

## arena mode

Follow `references/pstack-arena.md`. It names pstack files and skills. h00man is standalone, so use these in their place:

- Its platform-mapping links and the **poteto-mode** Subagents section: the rules under Models and harness above.
- **separate-before-serializing-shared-state**: `references/pstack-principle-separate-before-serializing-shared-state.md`.
- **redesign-from-first-principles**: `references/pstack-principle-redesign-from-first-principles.md`.
- **prove-it-works**: `references/pstack-principle-prove-it-works.md`. The **show-me-your-work** skill it names: `../build/references/show-me-your-work/SKILL.ref.md` when `build` is installed. Either way, keep the proof as a local file next to the artifact.
- The Laziness Protocol: `references/pstack-principle-laziness-protocol.md`.
- `/setup-pstack` in its Models section: not in the stack. Edit the role lines in `agent-models.md` by hand.

Read each principle file at the phase that names it.

## benchmark mode

Follow `modes/benchmark.md`. It is the user's own method. The words after `benchmark` are the argument its setup step parses, for example `/arena benchmark skill deep`.

## Sources

- h00man, by h00mankind (https://github.com/h00mankind/skills, MIT): the earlier `h00man:benchmark` skill, now `modes/benchmark.md`. It gives the benchmark mode: calibrated tasks, hermetic runs, the blind judge, and the report.
- pstack, by Lauren Tan (https://github.com/cursor/plugins/tree/main/pstack, ported at https://github.com/h00mankind/pstack, MIT): the `arena` skill, copied to `references/pstack-arena.md`, for the arena mode. The principle skills it names, copied to `references/pstack-principle-*.md`.
