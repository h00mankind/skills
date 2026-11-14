---
name: present
description: Build a single-file HTML artifact or a faux product UI to show something. Use for /present, an HTML page, an explainer, a plan, a comparison, a report, a post-mortem, slides or a deck, a diagram, a chart, a demo, a mocked UI, a product shot, or a cursor walkthrough animation. Skip chat answers and snippets the user pastes into an editor.
---

# Present

Replace a long markdown answer with one self-contained `.html` file. Plans, explainers, comparisons, maps, and post-mortems have a spatial structure that markdown flattens. A side-by-side layout shows a comparison, margin notes show a diff, and a clickable prototype shows an interaction.

## When

Make an HTML file when at least one of these is true. Otherwise, answer in chat.

- The answer **compares things** that the reader must point at.
- It has **spatial structure**: maps, timelines, flowcharts, diagrams.
- It has **interaction** that prose cannot show: easing, click-throughs, sliders, toggles.
- The user will **return to it or hand it off**: status reports, design systems, plans, post-mortems.
- The user will **edit it** as part of the work: boards, editors, tuners. Each of these ends with an export button.

The trigger is spatial structure or interaction, not length.

## Pick the path

1. **Faux product UI.** The user wants a mocked interface, a product demo, a hero or feature-card picture of an app, or an animated cursor in a UI. Use stageshow (see "Faux product UI").
2. **A chart or a dashboard of data.** Follow the Charts rules below before you write chart code.
3. **Everything else.** Use the plain HTML artifact path: the theme, the plumbing, and the catalog below.

A path can include another. For example, a report can hold a chart.

## Match the theme to the subject

The subject sets the look. Use no shared stylesheet. A post-mortem reads like a serious report: restrained, near-monochrome, one alarm color. An architecture map reads like a blueprint: monospace labels, thin rules. An explainer is editorial: serif headings, a generous measure. A triage board is a tool: dense and utilitarian.

- Define a token set inline in `:root`, and a `[data-theme="dark"]` override. Include colors (`--bg`, `--text`, `--muted`, `--border`, `--card`, `--accent`), radius, fonts, and the spacing scale. Use the tokens everywhere. Keep them the same inside one artifact. Change them between artifacts.
- Use one sans and one mono, and sometimes one serif. Pick by content:
  - **Tools, maps, dashboards, code review:** sans and mono, tight tracking.
  - **Reports, post-mortems, status:** a humanist sans and mono, neutral tracking.
  - **Explainers, research, learning:** serif headings, sans body, a generous measure.
  - **Decks, hero pages:** display serif headings, sans body.
- Refuse the default AI look. Inter with a purple gradient and identical rounded cards is not a choice. A clean technical pairing is fine when you pick it on purpose.
- Use an icon library, never unicode glyphs such as `★`, `⚠`, or `→`. The default is Remix Icon.

## Plumbing

Read `references/snippets.md` and paste from it. These parts are the same in every artifact. Theme only their tokens.

- **Spacing scale.** Set it once in `:root` and use it everywhere. A value outside the scale, such as `3px`, makes the layout look careless.
- **Theme switcher.** Ship both themes when the user returns to the artifact or reads it in different light.
- **Right-side TOC with scrollspy.** Add it for 3 or more `<h2>` sections.
- **Layout guardrails.** Always add the wrap, scroll, and `min-width: 0` CSS, so long URLs and shell lines stay inside cards and cells.

## Constraints

1. **One file.** Inline `<style>` and `<script>`. Use no build step and no npm. The file works when the user double-clicks it.
2. **CDN dependencies only, and few.** A font link is fine. Load a chart library from a CDN only when the chart needs it. Default to vanilla JS.
3. **State in JS variables.** Use browser storage only for small conveniences, such as the theme or a filter. Wrap each access in try/catch. The page must work without storage.
4. **Export.** An editor artifact gets a "copy as markdown" or "download JSON" button, so the edits go back into the next prompt.
5. **No server.** The file opens in a browser with no fetch to localhost.
6. **Clean prose.** Apply each rule in `references/pstack-unslop.md` to the text in the artifact.

## Where to save

1. Use the path the user named, if any.
2. In a git repo, use `<repo-root>/docs/html/`. Make the folder if it is missing.
3. Otherwise, use `~/Downloads/`.

Name the file with a kebab-case slug from the request, with no timestamp. End the turn with the absolute path and an `open` command on its own line.

## The catalog

Users say the phrase, not the pattern name. Map the request to one pattern, or to a planned combination. Do not put four patterns on one page.

1. **Exploration and planning.** "Compare these approaches", "draft a plan". Show options side by side with the trade-offs inline. Show a plan as a timeline, a dataflow diagram, and a risk table.
2. **Code review and understanding.** "Review this PR", "explain this package". Show a diff with margin notes and severity tags. Draw a module map with boxes, arrows, and the hot path marked.
3. **Design.** "Show me the tokens", "every state of the button". Make swatch sheets to copy from. Put variant grids on one sheet.
4. **Prototyping.** "What should this feel like". Make an animation sandbox with duration and easing sliders, or a clickable four-screen flow.
5. **Illustrations and diagrams.** "Diagram this pipeline". Draw inline SVG figures. Make flowcharts where a click on a step shows what runs and how it fails.
6. **Decks.** "Make slides from this". Use `<section>` elements with arrow-key navigation, in one file.
7. **Research and learning.** "Explain how X works here". Use a TL;DR box, collapsible steps, tabbed snippets, a hover glossary, and live mini-demos.
8. **Reports.** "Status update", "post-mortem". Show what shipped and what slipped, with a small chart. Show a minute-by-minute timeline with log excerpts and a follow-up checklist.
9. **Editing interfaces.** "Help me prioritize, toggle, or tune". Make a drag-to-rank board, a flag editor with dependency warnings, or a prompt tuner with live re-render. Each has an export button.

## Faux product UI

Read `references/stageshow/SKILL.ref.md` in full before you plan. Follow its workflow, its gold examples, and its rehearsal steps. Each path in that file, such as its `kit/` and `examples/` folders and its film script, is inside `references/stageshow/`.

- Copy `references/stageshow/` to a scratch folder before you run `npm install`, `npm run dev`, or the film script. Do not install packages into the skill folder.
- Copy `kit/` and the closest gold example into the user's project. The kit needs React, react-dom, and lucide-react.
- A plain HTML artifact can hold a stageshow still. Use the React kit only when the demo animates or the project is already React.

## Charts

Use these rules for every chart, even when the session has a chart skill from another source.

- Pick the form by the question: a line for change over time, a bar for a comparison, a dot plot for ranges. Do not use a pie chart for more than three parts.
- Use inline SVG for one or two charts. Use one CDN library for more.
- Label the axes and the units. Put a label on the line or bar, not in a far legend, when you can.
- Use one accent color for the series that the story is about. Use gray for the rest.
- Test the colors in both themes.

## Not in scope

- Animation rules belong to the `motion` skill.
- A framework is not the default. Use vanilla JS. Use React only when the interaction gains from it, or for the stageshow kit.

Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference names `SKILL.md`, read the `SKILL.ref.md` in that folder.

## Sources

- **h00man:html** by h00man23, https://github.com/h00mankind/skills, Apache-2.0. Takes the trigger rules, the theme choices, the plumbing in `references/snippets.md`, the constraints, the save path, and the catalog.
- **benji** (stageshow) by Benji Taylor, https://github.com/benjitaylor/skills, MIT (see `references/stageshow/NOTICE.md` for third-party marks and assets). Takes the whole stageshow folder: the skill, the React kit, the gold examples, the teaching pairs, and the film scripts for faux product UI.
- **pstack** by Lauren Tan (poteto), Claude Code port at https://github.com/h00mankind/pstack, MIT. Takes `unslop`, the numbered catalog of AI patterns to keep out of the artifact's text.
