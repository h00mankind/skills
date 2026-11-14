# The name artifact

How to build, fill, and verify the HTML artifact for the `name` skill. `SKILL.md` says when to read this file.

**Always make an HTML artifact**, in both modes. It is a **triage tool** that the user uses to compare, filter, and curate. It is not a report. Do not build it from a blank page. **Start from `template.html` in the skill folder, next to `SKILL.md`.** The template already has the layout, the AND filters, the single-`Set` state with saved stars, the shortlist rail, the data checks, and the clipboard logic with a fallback. These are the parts that go wrong on a blank page.

```
┌─────────────────────────────────────────────────┬────────┐
│  HEADER  project · brief in one line · mode      │        │
├─────────────────────────────────────────────────┤  SHORT │
│  1  COVERAGE BAR  per-type counts, click to filter│  LIST  │
│  2  FILTERS       [type] · [metaphor] · clear     │  PANEL │
│  3  TOP PICKS      ranked cards                   │ (right │
│  4  RECOMMENDED    scored cards, grouped by type  │  rail; │
│  5  THE POOL       compact chips (deep only)      │  below │
│                                                   │ on     │
│                                                   │ mobile)│
├─────────────────────────────────────────────────┴────────┤
│  FOOTER  path · how to copy out                           │
└───────────────────────────────────────────────────────────┘
```

## Fill the template

1. **Put in the data.** Replace `BRIEF` (the project, the one-line brief, `mode`, and the absolute path) and `CANDIDATES` with your real pool. Give each candidate `{ id, name, type, metaphor, gloss, sound, syllables, tier, score, rank, flag, check, rec }`. The comment block in the template defines each field. Set `rec: true` on your recommended shortlist. Those names start starred and show as cards. The rest go to the pool tier. Set `rank` on the top picks. Quick mode (`mode: "quick"`) hides the pool, so mark all of the about 20 names `rec: true`. Write the data as plain JSON-style literals. Never put a raw `</` in a string. Write `<\/`.
2. **Change the theme, not the structure.** Make the look fit the *subject*, as the h00man `present` skill does. Change the `:root` tokens: a finance tool does not look like a kids' app. Keep the layout and the JS engine. Restyle freely, but keep text contrast at 4.5:1 or better in both themes. Do not use the generic AI look (Inter with a purple gradient).
3. **Save and report.** Save one self-contained file to `<repo-root>/docs/html/` in a git repo. Make the folder when it does not exist. Outside a git repo, save to `~/Downloads/`. Report the absolute path and a command that opens it. Use plain Markdown only when the user asks for no artifact.

## Verify before the handoff

The template writes its own counts to `<body>`: `data-total` (candidates in the data), `data-rendered` (cards plus chips on screen), and `data-errors` (failed data checks, listed in a red banner). Read them with headless Chrome:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --dump-dom "file://$OUT" \
  | grep -oE 'data-(total|rendered|errors)="[^"]*"|<li>[^<]*</li>'
```

1. `data-errors="0"`. If not, the `<li>` lines name each bad row: a duplicate id or name, a `type` that is not in the type table, a `rec` that is not a boolean, or a pool row in quick mode. Fix the data and run it again.
2. `data-rendered` equals `data-total`. The usual bug is that only the shortlist shows, or the pool tier collapses.
3. In deep mode, no coverage row reads 0, unless you can say why that type does not fit.
4. Every top pick has a `check` note. The rest may read "not searched yet".

When a browser tool is available, also test the interactions. A star on a card and a star on a pool chip both update the rail. **Reset** brings back the default stars. **Clear** resets the filters. **Copy** gives Markdown. When the clipboard is blocked, **Copy** shows a selected text box.

