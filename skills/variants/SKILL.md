---
name: variants
description: Build 3 to 5 structurally different takes on a component, screen, or flow behind one picker, then recommend one. Has a low-fidelity mode (`/variants lofi`) that draws each take as a wireframe or diagram with Pele. Runs only when the user types /variants.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# variants

Your first idea is usually one you saw before. Make variants that differ in concept, not in styling. Then pick one.

## Reference files

- `references/matt-prototype/UI.md`: how to host variants on a real page behind `?variant=`, and the anti-patterns. Read it before step 3 when the project is an app.
- `references/emil-prototype/SKILL.ref.md`: the craft bar each variant must meet, the hard rules, and the `riff` and `keep` commands. Read it before step 2. Skip its "Initial Response" script. This skill replaces it.
- `references/emil-prototype/PICKER.md`: the picker markup, styles, and keys. Read it at step 3. Copy it as written.
- `references/lofi/LOFI.md`: low-fidelity mode. Each variant is a wireframe or a diagram (Mermaid source rendered by Pele), not working UI. Read it before step 3 when the request says `lofi`, wireframe, diagram, or sketch, or when the open question is structure or flow. In that mode, follow it in place of steps 3 and 5. An explicit low-fidelity request wins over the logic branch below, also for states.
- `references/matt-prototype/SKILL.ref.md` and `references/matt-prototype/LOGIC.md`: the logic branch. Use them only when the question is about state or logic, not looks. In that case, skip the steps below and build the logic prototype those files describe.

The included files name other skills. h00man is standalone, so never load one from another source. `review-animations` and `improve-animations` mean the h00man `motion` skill, if the project has it. `pick-ui-library` means the h00man `ui` skill, if the project has it. When the project has neither, skip that step and say so.

## 1. Say what varies

Write one sentence. Say what varies (component, screen, or flow) and what stays fixed (brand, data, core function). Ask the user only when you cannot tell the target. Do one piece per run. If the request spans many pieces, pick the one that matters most to the request and offer the rest as later runs.

## 2. Push for range, not depth

Make 3 variants by default and at most 5. Each must be a structurally different answer, not the same layout restyled.

Use this test. A numpad, a numpad with +/- steppers, and a numpad with preset chips are one idea with depth. A numpad, a slider, and a gamified picker are three ideas with range. Aim for range.

Pick the axes that differ for this case. Do not use all of them at once.

- **Density.** Compact and data-rich, or airy and focused.
- **Structure.** One view or steps. List, board, or timeline.
- **Emphasis.** Data first, action first, or explanation first.
- **Interaction.** Inline edit, modal, or a page of its own.
- **Tone.** Playful or sober. Editorial or utilitarian.

To find directions past the first one or two, apply at least two of these moves:

- **Remove or add a constraint.** What if this needed no screen? What if it ran on its own? (Manual photo backup becomes back up everything, and the interface goes away.)
- **Blend from another domain.** What if this were a game, or a physical product? What would Muji ship?
- **Invert the problem.** Help users rule out what they do not want, not pick what they want.
- **Maximize one quality.** Make one variant 10 out of 10 on a single quality (speed, trust, delight, access). Let that one goal decide the rest.
- **Force a count.** Write down 5 (or 12) directions before you judge any. The less obvious directions come late in the list.

Name each variant after its direction (`auto`, `slider`, `gamified`). Never use "Option A/B/C". If you cannot tell two variants apart in one sentence, they are one idea. Drop one and make a new one.

Before you write code, list each variant with its name and its axis. No two variants may share a position on one axis.

## 3. Build them all in one place

In low-fidelity mode, build the page that `references/lofi/LOFI.md` describes, and skip the rest of this step.

- **App project.** Put the variants on the real page that will host the piece, behind one URL parameter, as the UI file shows. Use a new throwaway route only when no page can host it. Use the project's components, tokens, and routing rules. Keep the picker out of production builds.
- **No app.** Make one self-contained HTML file with the same picker. Save it to `docs/html/<thing>-variants.html`.

Use the PICKER.md picker in both cases, not a picker of your own. PICKER.md keeps the current variant in `?v=`. Use `?v=` where the UI file says `?variant=`, so that the page has one parameter and a reload or a shared link keeps the variant.

Give every variant the same realistic data, so that the comparison is fair. Use mock data. Never connect a variant to real writes. Each variant must work, with real interactions, real motion, and real copy.

Run the result. Switch through every variant yourself. Confirm that each one renders, that each interaction responds, and that the console has no errors. Take a screenshot of each one if a browser tool is available.

## 4. Compare and recommend

Give a table with one row per variant: name, axis, when it is the right choice, and what it costs. Then recommend one, with a reason from the product and how often people use it. Take the best detail from the variants that lost. The winner is often the structure of one variant with a detail from another.

Then give the URL or file path, and the keys that switch variants.

## 5. Promote and clean up

When the user picks a variant, write it again in the real code, with tests and error handling. Do not ship the prototype code as it is. Follow the project's file layout and names. Then delete the variant files, the picker, and any throwaway route or HTML file, unless the user asks to keep them. Record the verdict (which variant won and why) in the commit or the PR.

For `riff <variant>` or `keep <variant>`, follow the emil SKILL.md table.

Request (if empty, the one the user is asking about in this conversation): $ARGUMENTS

Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference links to a `SKILL.md`, read the `SKILL.ref.md` in that folder.

## Sources

- h00man, by h00mankind (https://github.com/h00mankind/skills, MIT): the earlier `h00man:variants` skill. It gives the range axes, the moves that produce less obvious variants, the one-sentence test, and the recommend step.
- matt, by Matt Pocock (https://github.com/mattpocock/skills, MIT): the `prototype` skill, copied to `references/matt-prototype`. It gives the variants on a real page behind `?variant=`, the throwaway route as a last resort, and the anti-patterns.
- pele, by kepano (https://pele.run, MIT): the Mermaid renderer that low-fidelity mode loads from the CDN. `references/lofi/LOFI.md` is written here, from its docs and examples.
- emil, by Emil Kowalski (https://github.com/emilkowalski/skill, MIT): the `prototype` skill, copied to `references/emil-prototype`. It gives the picker, the craft bar, the named axis per variant, and the `riff` and `keep` commands.
