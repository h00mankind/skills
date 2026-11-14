---
name: ambitious
description: Turn a rough idea or draft into one ambitious, paste-ready prompt that another agent can run start to finish, in ask mode (the default) or rcmd mode (no questions, recommended guesses). Runs only when the user types /ambitious.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# ambitious

Turn a rough idea or draft into one prompt that an agent can run start to finish without help. The prompt states the goal behind the request and a proof of done from outside the conversation. It does not add scope. Run the work only if the user also asks for it, and only after the prompt.

## 1. Sharpen silently

Read `references/pstack-sharpen.md` and answer its four questions in your head. Skip everything after the four questions. Keep two things: the deeper goal, and a proof of done from outside you and the user. Do not load a `sharpen` skill from another source.

## 2. Guess the four points

1. **Goal.** What, and for whom, in one line.
2. **Done.** The check that proves it, and the next measurable target after it passes. Decide if hill climbing fits: big, tedious work that a check can score, judge scores included. Fuzzy work needs a written checklist and reference screenshots, because the judge is the weakest part. If the user gives none, the prompt has its agent draft the checklist and collect references first, and log them as assumptions.
3. **Scope.** What is in scope, and what the agent may touch.
4. **Stop rule.** N attempts in a row with no improvement, plus a generous attempt cap and hour cap.

**Ask mode** (the default): ask the four points once, hill climbing included, and for fuzzy work, checklist points and reference screenshots, with the question tool if the harness has one (`AskUserQuestion` in Claude Code), else in one message. Put your guess first in each question, marked recommended, with its reason, because the dialog can hide earlier text. Write nothing until the user answers.

**Rcmd mode**: use it when the request starts with `rcmd` (drop that word) or the user says to go with your guesses. Ask nothing, and use your guesses.

In rcmd mode, or when the user takes every guess unchanged, put the riskiest guesses on one `Assumed:` line in Context.

## 3. Write the prompt

Write it yourself, in the user's voice (I, my). Use English unless the user asks otherwise, even if they wrote in another language. Aim for 100 to 150 words, 200 at most. Use these five sections and nothing else:

- **Goal:** one or two lines.
- **Context:** only facts, inputs, and paths the agent cannot find itself, a fallback for each missing input, the checklist and references a judged check needs, and the `Assumed:` line, if any.
- **Done when:** the automated check: tests, parity, a benchmark, or a judge panel against a written checklist. Add what must not get worse. If hill climbing fits, add each next target on its own line. Accept a change only if every earlier check still passes. End on the best passing version.
- **Limits:** hard constraints, anti-goals, and what not to touch.
- **Run rules:** one line. Don't ask questions. Log assumptions and blockers to files and keep going. No destructive, irreversible, paid, or secret-touching actions. Stop after N attempts in a row with no improvement, or at the attempt cap or hour cap.

Give outcomes, not instructions: no steps, methods, log formats, or edge cases. Exact details of the check are fine. Name no model, CLI, or tool. Keep each user answer with its exact numbers. Cut what the agent would do anyway. Apply `references/pstack-unslop.md`.

## 4. Output

Give one fenced code block with the prompt. Add nothing else unless the user asks or a handoff needs a named edit. On pushback, change the same prompt. Redo section 2 only if the goal changes.

If the user asks you to send it to a teammate, send their exact instruction (project, folder, thread) and the prompt word for word, once, to the named bot. The only allowed edit makes a location line match the destination. Name that edit.

Request (if empty, the one the user is asking about in this conversation): $ARGUMENTS

## Sources

- h00man, by h00man23 (https://github.com/h00mankind/skills, Apache-2.0): the earlier `h00man:ambitious` skill. It gives the ask-once step, the five-section recipe, and the handoff.
- pstack, by Lauren Tan (https://github.com/cursor/plugins/tree/main/pstack, ported at https://github.com/h00mankind/pstack, MIT): the `sharpen` skill, copied to `references/pstack-sharpen.md`, gives the four questions that find the deeper goal. The `unslop` skill, copied to `references/pstack-unslop.md`, gives the catalog of AI patterns to keep out of the prompt.
