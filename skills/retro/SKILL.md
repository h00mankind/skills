---
name: retro
description: Look back on agent work and stop its mistakes from coming back. Guard each repeated mistake with a check that fails on its own, keep judgment calls in TASTE.md, and propose environment and skill fixes. Runs only when the user types /retro.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# retro

A retro turns past agent mistakes into changes that stop the next agent from repeating them. Follow the user's guardrails method first. A mistake that a check can catch gets a check. Only a judgment call gets text, in `TASTE.md`. The other sources add more evidence and more kinds of fixes.

## Pick the mode

The first word of the argument picks the mode. The rest of the argument names the session or the focus.

- **session** (the default): look back on one session. Use this session unless the user names another.
- **reflect**: the session mode with a panel of three reviewers and a synthesizer in place of your own read. Use it for a long or contested session.
- **project**: the full guardrails audit of the project's history since the last audit. Use it for a new repo, or when no audit ran for a while.

## Files

Read `references/guardrails/SKILL.ref.md` first in every mode. It gives the bar a fix must clear, the five levels, the steps, and the reply. The other files serve it.

- `references/guardrails/references/ui.md`: the UI pack. Its step 2 names it by a path relative to its own folder.
- `references/emil-animation-audit.md`, `references/emil-animation-standards.md`, and `references/emil-break-ui-catalog.md`: the three emil problem lists that its step 2 names. Never load the `emil` plugin, even when the session has it, and ignore the "if the `emil` plugin is installed" test in guardrails. Read these copies when the project has the matching surface.
- `references/matt-retro.md`: the kinds of environment fix (navigation, checks, steering files, tool economy, no-ops, information access) and which file each kind of text belongs in.
- `references/reflect/SKILL.ref.md`: the reviewer panel, its prompt templates, and the transcript finder.
- `references/pstack-correct.md`: an older form of the guardrails ladder. On any conflict, follow guardrails. Read it only to compare.

## session mode

1. **Find the session.** For a Claude Code session, run the finder from step 1 of `references/reflect/SKILL.ref.md`. Its scripts are in `references/reflect/scripts/` in this skill's folder. For another agent, read its log folder that guardrails step 3 names.
2. **Collect evidence.** List each correction from the user in their own words. Add each mistake the agent caught and fixed itself. Add each slow or costly step.
3. **Sort each finding** into one kind:
   - **Mistake.** Wrong work that the user or a check caught.
   - **Friction.** Time or tokens lost to navigation, missing information, costly tool calls, or steering text that changed nothing. Use the kinds in `references/matt-retro.md`.
   - **Skill learning.** A skill gave wrong or missing guidance, or did not trigger when it should have.
4. **Guard the mistakes now.** Take each one through guardrails steps 4 to 6. One session is often one piece of evidence. Search `GUARDRAILS.md`, `TASTE.md`, and the git history for a second piece before you file it under Candidates.
5. **Propose the rest.** Show friction and skill learnings as proposals, the most severe first. Make no change to a skill or an agent instruction file until the user approves it.

## reflect mode

1. Do steps 1 to 3 of `references/reflect/SKILL.ref.md`: find the transcript, run the three reviewers, and run the synthesizer.
2. Send each Backlog item, and each Accepted item that a check could enforce, through guardrails steps 4 to 6 in this run. Do not file them to a tracker.
3. Show the remaining Accepted and Rejected lists. Apply an Accepted skill edit only after the user approves it, as its step 5 says.

## project mode

Run guardrails steps 1 to 6 as written. When the evidence also shows friction, add those findings as proposals, as in session mode step 5.

## Names the included files use

The included files name skills and files that this skill set does not include. Use these in their place:

- `/guardrails` and `/reflect`: this skill. `/reflect` is reflect mode.
- `/setup-pstack`: not in the stack. Edit `agent-models.md` by hand, or use the defaults in the reflect file.
- `writing-for-agents` and `plugin-dev:skill-development`: the h00man `writing` skill when it is installed. Otherwise follow the format of the skills that are already in the repo.
- **encode-lessons-in-structure**: the guardrails ladder.
- The **poteto-mode** Subagents section and the platform-mapping links: the harness's own subagent tool. Set each reviewer's effort for its job. Take models from `agent-models.md` in `${CLAUDE_CONFIG_DIR:-$HOME/.claude}/` when it exists, else from the defaults in the reflect file. Route each one with the Routing rules in the Models section of `../build/SKILL.md`.
- `CODING_STANDARDS.md`: `TASTE.md`.
- The rule table in the agent instruction file: the rule rows in `GUARDRAILS.md`.
- `<plugin>/skills/reflect/`: `references/reflect/` in this skill's folder.
- A skill from this set: propose the edit to the user. Do not edit an installed copy without their approval.

## Reply

Give the guardrails reply first. Then list the friction proposals and the skill-edit proposals. Mark each one that waits for the user's approval.

## Sources

- h00man, by h00man23 (https://github.com/h00mankind/skills, MIT): the `h00man:guardrails` draft, copied to `references/guardrails/`. It is the main method. It gives the bar, the five levels, the steps, `GUARDRAILS.md`, `TASTE.md`, and the UI pack.
- matt, by Matt Pocock (https://github.com/mattpocock/skills, MIT): the `retro` skill, copied to `references/matt-retro.md`, for the kinds of environment fix and where each kind of text lives.
- pstack, by Lauren Tan (https://github.com/cursor/plugins/tree/main/pstack, ported at https://github.com/h00mankind/pstack, MIT): the `reflect` skill, copied to `references/reflect/`, for the reviewer panel and the transcript finder. The `correct` skill, copied to `references/pstack-correct.md`, as the older form of the guardrails ladder.
- emil, by Emil Kowalski (https://github.com/emilkowalski/skill, MIT): the problem lists from `improve-animations`, `review-animations`, and `break-ui`, copied to `references/emil-*.md`, for the guardrails packs.
