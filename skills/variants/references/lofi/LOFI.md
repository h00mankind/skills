# Low-fidelity mode

Low-fidelity mode shows each variant as a wireframe or a diagram, not as working UI. Each variant is Mermaid source that [Pele](https://pele.run) renders as SVG. Use it to compare structure before anyone spends time on looks, motion, or code.

## When to use it

- The user asks for wireframes, a diagram, a sketch, or low fidelity (`/variants lofi <thing>`).
- The question is about structure, flow, or states, and pixels would hide the difference.
- The variants are big enough that 3 working builds cost too much for a first pass.

Steps 1 and 2 of the skill do not change. The range rules, the axes, the moves, and the one-sentence test apply to diagrams too. Two wireframes with the same boxes in a different color are one idea.

## Pick the diagram type

Pick the type from what varies. All variants in one run use the same type, so that the comparison is fair.

| What varies | Pele type | Header |
| --- | --- | --- |
| Screen or component layout | Block (wireframe) | `block` |
| Steps, branches, decisions | Flowchart | `flowchart LR` |
| States of one thing | State | `stateDiagram-v2` |
| Calls between user, app, and services | Sequence | `sequenceDiagram` |
| How the experience feels over time | User journey | `journey` |
| Navigation or information architecture | Mindmap or tree view | `mindmap`, `treeView-beta` |
| Who does what, in which lane | Swimlane | see pele.run/examples |

For other types (timeline, kanban, quadrant chart, and more), copy the syntax from https://pele.run/examples. Pele reads Mermaid syntax.

## Wireframes with `block`

A wireframe is a grid of labeled boxes. `columns N` sets the grid. `id["label"]:N` spans N columns. `space` leaves a gap. Shapes carry meaning: `[" "]` is a region, `(["label"])` is a button, `[("label")]` is stored data.

```
block
  columns 3
  hdr["Header: logo, search, avatar"]:3
  nav["Nav: 5 links"] list["Orders: 8 rows, status chip, total"]:2
  space pay(["New order"]) filter(["Filter"])
  classDef primary stroke-width:3px
  class pay primary
```

- Label each box with its content and count ("Orders: 8 rows, status chip, total"), never "Content" or lorem ipsum.
- Mark the one primary action with the `primary` class. No other color.
- Keep one screen per diagram. When a variant has steps, use a flowchart of screens, or one block diagram per step stacked in the same variant.

## Build the page

Make one self-contained HTML file at `docs/html/<thing>-variants-lofi.html`. Use it in an app project too: low-fidelity variants never go into the app.

- Load Pele from the CDN with an exact version: `<script src="https://cdn.jsdelivr.net/npm/pele@1.12.0/dist/pele.global.js"></script>`.
- Keep each variant's source in `<script type="text/plain" data-variant="<name>">`.
- Put the PICKER.md picker on the page. Copy its markup, styles, and keys as written. It keeps the variant in `?v=`. Leave out the replay button, since diagrams have no motion.
- Replace only the picker's `mount(i)` function. Call `pele.mount(stage, source)` once for the first variant and keep the result. After that, `mount(i)` calls `update(source)` on that result, so that the stage is not cleared on a switch.
- Above each diagram, show the variant name and its one-sentence direction. Below it, put a `<details>` with the Mermaid source, so the user can copy it to the Pele playground.
- Use a grayscale theme so that looks do not sway the choice: set `--pele-surface`, `--pele-border`, `--pele-line`, and `--pele-fg` to grays, and redefine them under `prefers-color-scheme: dark`. Set `--pele-font` to the page font.
- Show a Pele error with `textContent`, never `innerHTML`. `mount()` shows its own errors in the container.

## Check it

Render every source before you open the page. A Mermaid syntax error is the common failure.

```sh
cd "$(mktemp -d)" && npm init -y >/dev/null && npm i pele@1.12.0 >/dev/null
node --input-type=module -e "import {render} from 'pele'; render(process.argv[1])" "<source>"
```

A `PeleError` names the line. Fix it and render again. Then open the page, switch through every variant, and confirm that each diagram shows and that the console has no errors.

## Compare and promote

Step 4 is the same: a table, a recommendation, and the best details from the variants that lost. Then offer the next step. A low-fidelity winner is a structure, not a build. Run `/variants` again at full fidelity on that structure (vary looks and interaction, keep the structure fixed), or build it directly when no open question is left. Keep the HTML file until the user picks a variant and that step is done. Then delete it, unless the user asks to keep it.
