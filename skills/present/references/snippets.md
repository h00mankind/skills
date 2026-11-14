# present: paste-in plumbing

These parts of an artifact are the same every run: the spacing scale, the icon webfont, the theme switcher, the TOC with scrollspy, and the layout guardrails. Copy them as written. They carry no personality, so do not redesign them. `SKILL.md` holds the choices. This file holds the boilerplate.

## Spacing & layout scale

Set the scale once in `:root`. Use it for every gap, padding, and stack rhythm. Do not hard-code pixel or em spacing. If you need `padding: 3px`, use a `--space-*` token, or extend the scale once. One stray `3px` breaks the rhythm.

```css
:root {
  --space-0: 2px;  --space-1: 4px;  --space-2: 8px;  --space-3: 12px;
  --space-4: 16px; --space-5: 24px; --space-6: 32px; --space-7: 48px;
  --space-8: 64px; --space-9: 96px;
  --measure: 68ch;   /* prose width cap */
  --radius: 6px;
  --tracking-tight: -0.022em;
  --tracking-body: -0.011em;
}
```

Use `--space-7` to `--space-8` for section gaps. Use `--space-4` to `--space-5` for card padding. Cap prose at `--measure`. Do not let cards or tables stretch from edge to edge in a wide viewport.

## Icon webfont (Remix Icon)

Remix Icon is a webfont. It needs one CSS link, `<i class="ri-…">` markup, and **no JS init call**. So an icon cannot fail to render after a DOM update.

```html
<link href="https://cdn.jsdelivr.net/npm/remixicon@4.5.0/fonts/remixicon.css" rel="stylesheet">
<button><i class="ri-star-line"></i></button>
```

Icons come in `-line` and `-fill` pairs, which suit toggles. Common icons: `ri-star-line`/`ri-star-fill`, `ri-error-warning-line`, `ri-check-line`, `ri-close-line`, `ri-clipboard-line`, `ri-arrow-go-back-line` (reset/undo), `ri-arrow-down-s-line`, `ri-sun-line`/`ri-moon-line`, `ri-download-line`, `ri-external-link-line`, `ri-arrow-right-line`. Set the size with `font-size` and the color with `color`. Icons inherit these like text (`i { font-size: 14px; line-height: 1 }`).

To swap an icon at runtime, change the class. No re-init is necessary:

```js
function setIcon(el, name) { el.className = name }   // setIcon(themeIcon, dark ? 'ri-sun-line' : 'ri-moon-line')
```

A webfont is better than an SVG-injection library such as Lucide. Lucide *replaces* each `<i data-lucide>` with an inline `<svg>`. After the first `createIcons()`, a query for the `<i>` fails, and simple toggle code stacks duplicate icons. You can use Phosphor or Heroicons when the mood needs them. Use one library per artifact. Inline SVG is fine for 2 or 3 icons.

## Theme switcher

Put a button at the top right with a **text label only** ("Dark mode" or "Light mode"). Do not use sun or moon glyphs. They render differently on each system and look like placeholders. Define both palettes in `:root` and `[data-theme="dark"]`. Flip `data-theme` on `<html>`. Keep the current theme in a JS variable. Save it only as Constraint 3 allows. Follow `prefers-color-scheme` on first paint, and sync the label then.

```html
<button id="theme" aria-label="Switch theme">Dark mode</button>
<script>
  const html = document.documentElement
  const btn = document.getElementById('theme')
  const syncLabel = () => btn.textContent = html.dataset.theme === 'dark' ? 'Light mode' : 'Dark mode'
  if (matchMedia('(prefers-color-scheme: dark)').matches) html.dataset.theme = 'dark'
  syncLabel()
  btn.onclick = () => { html.dataset.theme = html.dataset.theme === 'dark' ? 'light' : 'dark'; syncLabel() }
</script>
```

## Right-side TOC + scrollspy

Add it to long artifacts with 3 or more `<h2>` sections. Put it on the **right**, not the left. The reader looks at the content, and the TOC stays at the side.

**Put the header and footer outside the grid**, so that their border spans the full page width. The 2-column grid wraps only `<main>` and `<aside>`. If it wraps the header too, the header rule stops at the content column and leaves a divider next to the TOC. Use `minmax(0, 1fr)` for the content, `220px` for the TOC, and a gap of `--space-7`. The `minmax(0, ...)` keeps code blocks inside the column. Below 960px, hide the TOC. Do not add an "On this page" label. The position and shape make the purpose clear.

**Get the tick gap right.** Put the vertical spacing on the `<li>`, outside the tick. Draw the tick with a `::before` that has the height of the link only. If you do not, the ticks meet and look like one continuous rail. The active state sets the pseudo-element to `--text` and the link weight to `600`. The tick is 2px. Do not make it wider when active. The color shows the state.

```html
<div class="page">
  <header>…<button id="theme">Dark mode</button></header>
  <div class="layout">
    <main>…<section id="…">…</section>…</main>
    <aside class="toc">
      <ul>
        <li><a href="#a">Section A</a></li>
        <li><a href="#b">Section B</a></li>
      </ul>
    </aside>
  </div>
  <footer>…</footer>
</div>
```

```css
.toc li { padding: var(--space-2) 0; }      /* gap lives on the li, outside the tick */
.toc a {
  display: block; position: relative;
  padding-left: var(--space-4); color: var(--muted);
}
.toc a::before {                             /* the tick, only as tall as the link */
  content: ''; position: absolute;
  left: 0; top: 0; bottom: 0; width: 2px;
  background: var(--border);
}
.toc a.active { color: var(--text); font-weight: 600; }
.toc a.active::before { background: var(--text); }
```

For the scrollspy, wrap each `<h2>` and its content in a `<section id>`. Observe the **sections**, not the headings. A tight `rootMargin` on headings changes the active link while the reader is still in the middle of a section. Highlight the section with the largest visible area.

```js
const links = new Map([...document.querySelectorAll('.toc a')].map(a => [a.hash.slice(1), a]))
const ratios = new Map()
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => ratios.set(e.target.id, e.intersectionRatio))
  const top = [...ratios.entries()].sort((a, b) => b[1] - a[1])[0]
  links.forEach(a => a.classList.remove('active'))
  if (top && top[1] > 0) links.get(top[0])?.classList.add('active')
}, { threshold: [0, .25, .5, .75, 1] })
document.querySelectorAll('main section[id]').forEach(s => io.observe(s))
```

## Layout guardrails

Long URLs and shell commands can overflow cards and table cells. Always include this CSS:

```css
:not(pre) > code { overflow-wrap: anywhere; word-break: break-word; }  /* inline code wraps */
pre { overflow-x: auto; max-width: 100%; }                              /* block code scrolls */
pre code { overflow-wrap: normal; word-break: normal; white-space: pre; }
.card, .sidebar, .grid > * { min-width: 0; }                            /* flex/grid children can shrink */
```
