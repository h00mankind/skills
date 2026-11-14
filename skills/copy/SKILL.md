---
name: copy
description: Write or revise product and marketing copy. Use for a landing page, a headline, a tagline, a CTA, button labels, UI text, error messages, empty states, tooltips, or onboarding. Skip docs, READMEs, and commit or PR text (writing).
---

# Copy

Write words that help the user do the task. Cut each string that does not.

## Scope

- Website and landing-page text: headlines, subheads, feature lines, CTAs.
- Product UI strings: buttons, labels, errors, empty states, tooltips, onboarding.

Docs, READMEs, ADRs, commit messages, and PR text go to the `writing` skill. A product or company name goes to the `/name` command.

## Voice

Read the existing copy first: the site, the app strings, and the README. Match it.

If the product has no copy yet, use this default voice: plain, specific, and confident. Use sentence case. Use no exclamation marks.

## Rules

- **Lead with the benefit or the action.** Do not lead with the feature or the system. "Get paid faster" is better than "Invoice automation platform".
- **Make each button a verb.** It finishes the phrase "I want to...". Write "Save changes" or "Send invoice". Never write "Submit", "OK", or "Yes".
- **Give each concept one name everywhere.** A "project" on one screen is not a "workspace" on the next screen.
- **Cut the filler.** Remove "please", "simply", "just", "in order to", and stacked marketing adjectives.
- **Write errors in two parts.** Say what happened. Then say what to do next. Never blame the user. Never discard the user's input.
- **Use the empty state to teach.** Say what belongs here and how to add the first one.
- **Use specifics, not adjectives.** "Loads in 50ms" is better than "Blazing fast".

## Process

1. **See the string in context.** Find the screen, the place in the flow, and what the user tries to do at that moment.
2. **Draft options.** Write 2 or 3 options for high-stakes strings: headlines, CTAs, and error messages. One option is enough for a trivial label.
3. **Cut the AI tells.** Read `references/pstack-unslop.md`. Remove each pattern it lists from your draft. Cite the rule number when you explain a cut.
4. **Show the copy in context.** Do not return a bare list. Mock up the sentence or the screen around the string.
5. **Read it aloud.** If it sounds stiff when spoken, write it again.

## Done

- Each string matches the product's voice and its names for things.
- Each button is a verb, and each error says what to do next.
- No pattern from `references/pstack-unslop.md` remains.
- The user sees each high-stakes string in context, with its options.

## Sources

- **h00man:copywriting** by h00man23, https://github.com/h00mankind/skills, Apache-2.0. Takes the voice default, the rules, and the process for product copy.
- **pstack** by Lauren Tan (poteto), Claude Code port at https://github.com/h00mankind/pstack, MIT. Takes `unslop`, the numbered catalog of AI patterns to cut from a draft.
