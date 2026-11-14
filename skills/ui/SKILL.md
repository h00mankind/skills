---
name: ui
description: Design, build, or review the look of an interface. Sets a visual direction, then works on layout, type, color, spacing, and components. Picks a UI library, stress-tests a screen with worst-case data, makes a web app feel native on a phone, and cleans up UI that looks AI-generated. Use when the user asks to design a page or screen, build a component, polish or redesign UI, pick a UI library (for example Radix, shadcn, Mantine, or MUI), "unslop" a screen, "make it look less generic", or "try the worst case". For animation and gestures, use motion. For the words in a UI, such as labels, errors, and empty states, use copy.
---

# ui

Each screen must look like it belongs to this product and no other. Make each size, color, and word a choice, not a default. Be bold in one place and keep the rest plain.

## Pick the job

Find the job in the request, then read the files for that job. Each file under `references/` is an upstream skill copied verbatim. Read its SKILL.md first, then the files it links.

- **New UI or a new direction.** Read `references/frontend-design/SKILL.ref.md`. Take the subject-first direction, the two-pass plan (tokens, then a review against the brief), and the list of default looks to avoid.
- **Build or refine components.** Read `references/emil-design-eng/SKILL.ref.md`. Take the component rules and the review checklist. Its motion sections belong to the motion skill. Follow that skill when it is installed.
- **Apple-style feel.** Read `references/apple-design/SKILL.ref.md` for materials, depth, typography (optical size, tracking, leading), and the design foundations. For its gesture and spring sections, use the motion skill.
- **AI-looking UI.** Read `references/unslop/SKILL.ref.md` and run its passes in its order. Keep the features, content, and section order. Change only the craft.
- **Pick a library.** Read `references/pick-ui-library/SKILL.ref.md`. Check `package.json` first. Recommend one library.
- **Toasts with Sonner.** Read `references/ask-sonner/SKILL.ref.md`, and `references/ask-sonner/API.md` for exact props.
- **Worst-case data.** Read `references/break-ui/SKILL.ref.md` and `references/break-ui/CATALOG.md`. Report what breaks before you fix it.
- **Web app on a phone.** Read `references/mobile-native/SKILL.ref.md` for the CSS and meta-tag fixes. Test on a real device.
- **Project uses @fluid (Fluid Functionalism).** Read `references/fluid-functionalism/SKILL.ref.md` before you install, compose, or style anything. Install from the registry. Do not rebuild a component that the registry ships.

A request can need more than one job. Do them in this order: direction, structure, components, details, stress test.

## Order of work

1. **Read the project.** Find the stack, the UI library, the tokens, and the theme. Use what exists before you add something new.
2. **Set the direction.** Name the subject, the audience, and the main job of the screen. Write a short token plan: 4 to 6 colors, one or two typefaces, the layout, and one memorable element. If the user gave a direction, follow it exactly.
3. **Check the plan.** Ask if the plan could fit any other product. If yes, change that part and say what you changed.
4. **Build.** Use real content from the product. Do not use lorem ipsum.
5. **Look at it.** Take a screenshot at desktop and at phone width. Check light and dark themes, keyboard focus, and contrast. A screenshot shows faults that you cannot see in the code.
6. **Stress it.** For a screen that renders data, run the worst-case pass from `break-ui`.
7. **Cut one thing.** Remove one decoration that does not serve the user.

## Choices only h00man makes

- **One source of truth for a rule.** When two files disagree, follow the more specific file. The order is: the user's words, then `fluid-functionalism` in a project with @fluid, then `unslop` for cleanup work, then `frontend-design` for new direction, then `emil-design-eng`.
- **Follow Fluid Functionalism for its components.** In a project with @fluid, its motion tokens and hover patterns are fixed. Do not apply other spacing or radius rules on top of a registry component.
- **Do not use a default AI look unless the user asks for it.** These looks include a warm cream page with a serif and a clay accent, an all-caps eyebrow over each heading, identical rounded cards, and gradient text. Both `frontend-design` and `unslop` list more.
- **Copy in the UI.** Use sentence case and active verbs. A button says what it does. For longer product copy, use the copy skill.
- **Library picks come from the list.** Use a library outside `pick-ui-library` only when the list does not cover the task. Say so when you do.
- **Read-only reviews stay read-only.** When the user asks for a review, report findings in the format from `emil-design-eng`. Do not edit code until the user asks.

## Upstream names in these files

The included files name other skills and paths. Map them as follows:

- `<skill>` and `<skill folder>` in a script command mean the folder of that file. Run each script from the project root. Give the script by its absolute path in this skill folder. For example, run `node <this skill folder>/references/fluid-functionalism/scripts/stack.mjs`, and `node <this skill folder>/references/unslop/scripts/snapshot.mjs` for a live page.
- `animate`, `animate-expo`, `gestures`, `review-animations`, `improve-animations`, and the other animation skills are in the motion skill. If motion is not installed, follow the motion rules in `emil-design-eng`.
- `emil-design-eng`, `apple-design`, `pick-ui-library`, `ask-sonner`, `break-ui`, and `mobile-native` mean the folders of the same name under `references/`.
- `prototype` is the h00man `variants` command. Tell the user to run `/variants`. Do not invoke it yourself.
- Each emil file starts with an "Initial Response" section. Ignore it. Do the task.

## Sources

- **anthropic**: `frontend-design` by Anthropic, https://github.com/anthropics/skills, Apache-2.0. Takes visual direction, the two-pass plan, and the list of default looks.
- **emil**: `emil-design-eng`, `apple-design`, `pick-ui-library`, `ask-sonner`, `break-ui`, and `mobile-native` by Emil Kowalski, https://github.com/emilkowalski/skill, MIT. Takes component craft, Apple-style materials and type, library picks, Sonner setup, worst-case data tests, and mobile web fixes.
- **heydamir**: `unslop` by Damir Sokolovsky, https://github.com/heydamir/unslop, CC BY 4.0. Takes the passes that turn AI-looking UI into a designed interface. The copy keeps the author's credit.
- **mickadesign**: `fluid-functionalism` by Micka Touillaud, https://github.com/mickadesign/fluid-functionalism, MIT. Takes the @fluid component catalog, craft notes, and stack scripts.
