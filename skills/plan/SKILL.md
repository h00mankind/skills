---
name: plan
description: Plan a feature into a PRD with vertical-slice issues in docs/plans, in grill mode or trust mode. Runs only when the user types /plan.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# plan

Turn an idea into a PRD and a set of vertical slices.

## Pick the mode

The first word of the argument picks the mode: `/plan grill` or `/plan trust`. The rest of the argument is the idea.

- **Grill mode.** Interview the user first, then write the plan. Use it when the idea is fuzzy, decisions are open, or the user wants you to challenge their thinking.
- **Trust mode.** Ask nothing. Build the plan from the conversation, the code, and the knowledge base. Use it when the context is already rich or the user wants speed.

With no mode word, recommend one mode in one line and wait. Base the choice on how much context exists. For example: "Context is thin, so grill mode fits. Say *trust* to skip the questions."

## Knowledge base

Read the project's existing glossary. If the project has none, use `docs/CONTEXT.md`. Read the ADRs in `docs/adr/` for each area the plan touches. Follow the decisions in those ADRs.

Keep both files current as decisions settle, in both modes:

- Follow `references/domain-modeling/SKILL.ref.md` to challenge fuzzy terms and to decide when a decision needs an ADR.
- Take the entry formats from `references/domain-modeling/GLOSSARY-FORMAT.md` and `references/domain-modeling/ADR-FORMAT.md`.
- Where that skill names `GLOSSARY.md`, use that glossary file. `build` uses the same file.
- Write each term and each ADR when it settles. Do not save them for the end.

## Grill mode

Run the interview in `references/matt-grilling.md`. Apply the domain-modeling rules during the whole interview.

`references/matt-grill-with-docs.md` pairs those two skills. Where it says to call the Skill tool, read the two files above. Never load a skill of that name from another source, even when the session has one.

Look up facts in the code yourself. Put only decisions to the user. The interview ends when the frontier is empty or the user says "done". The settled decisions go into the PRD.

## Trust mode

Work only from the conversation, the code, and the knowledge base. Do not ask. Write each assumption in the Decisions section. Use the glossary terms. Respect the ADRs. Put a real blocker in Open questions. Do not guess at it.

## The PRD

Write it to `docs/plans/NNNN-<slug>.md`. `NNNN` is four digits with leading zeros, one more than the highest number in `docs/plans/`. The first plan in a repo is `0001`. This sequence is separate from `docs/adr/`.

Use these sections:

- **Problem.** The problem from the user's side, not the system's.
- **Solution.** What changes for the user.
- **User stories.** A long numbered list: "As a <actor>, I want <feature>, so that <benefit>."
- **Decisions.** Modules touched, schema and API contracts, and settled trade-offs, in glossary terms. Mark each one as grilled or assumed. Use no file paths and no code. The one exception is a snippet that a prototype proved and that states a decision better than prose.
- **Open questions.** Each open point, so that none is lost.
- **Out of scope.** Say it explicitly.

Apply each rule in `references/pstack-unslop.md` to the PRD and the slices.

## Slice

Cut the work into vertical slices. Each slice goes through every layer (schema, API, UI, tests) and can be demoed alone. Prefer many thin slices to a few thick ones.

Give each slice a title, the behavior to build (not the layers), acceptance criteria as checkboxes, and what blocks it.

In grill mode, show the slices and change them until the user approves. In trust mode, deliver them. The user edits after.

## Publish

By default, the slices stay in the PRD as sections with checkboxes. If the repo has a GitHub remote and the user wants issues, file them with `gh issue create`. File blockers first, so each "Blocked by" names a real issue number.

## Sources

- h00man, by h00man23 (https://github.com/h00mankind/skills, Apache-2.0): the earlier `h00man:plan` skill. It gives the two modes, the knowledge base rules, the PRD sections, the slices, and the publish step.
- matt, by Matt Pocock (https://github.com/mattpocock/skills, MIT): the `grilling` skill, copied to `references/matt-grilling.md`, for the interview in rounds. The `grill-with-docs` skill, copied to `references/matt-grill-with-docs.md`, for the pairing of grilling with domain modeling. The `domain-modeling` skill, copied to `references/domain-modeling/`, for term challenges, ADR rules, and the entry formats.
- pstack, by Lauren Tan (https://github.com/cursor/plugins/tree/main/pstack, ported at https://github.com/h00mankind/pstack, MIT): the `unslop` skill, copied to `references/pstack-unslop.md`, for the numbered catalog of AI patterns to keep out of the PRD.
