# Audit and improve

Find what is weak, prove it, then make it better. This is the h00man audit: severity-ranked findings, then fixes when the mode allows them. It was the `improve` skill.

## Area, mode, and scope

Settle three things before you start:

1. **Area.** `ui`, `ux`, `code`, `security`, or `all` (deep). Deep runs every area and adds an architecture pass.
2. **Mode.** **Audit** reports findings and changes nothing. **Execute** audits, then applies every safe fix in one go, with no second confirmation.
3. **Scope.** A page, a flow, a module, a branch, or a diff. Without one, use the current diff or the most recent work, and state that assumption.

Infer each one from the request when you can. "Is this safe to ship" is `security` plus `code` in audit mode. "Clean this up" is execute mode. Ask only for what you cannot infer, and ask for all of it in one question.

## Process

1. **Inspect the real thing.** For `ui` and `ux`, look at the rendered result, not the source. See *Seeing the UI*. For `code` and `security`, read the actual call paths, not only the files that changed.
2. **Sweep with the matching checklist** below. Then go past it: cross-cutting patterns, inconsistencies between areas, and problems the checklist does not name.
3. **Critique like a skeptical reviewer.** A checklist sweep finds the obvious problems. The findings that matter come from a second, adversarial read. See *The critique pass*.
4. **Report** in the format below.
5. **Execute mode:** apply the fixes at once, with the smallest diff per finding and no change in behavior. This raises quality, not functionality. Verify after: inspect the UI again, walk the flow, run the code and the tests. A finding that is too risky to patch inline (an auth-model change, a migration rewrite, a large architecture refactor) stays report-only. Say so explicitly. **Audit mode:** stop after the report. End with an offer to apply the fixes in execute mode.

### Do not flatten creative intent

Accessibility has two parts. The **floor is mandatory and never skipped**: semantic elements, a visible focus state (any style), keyboard reach, and contrast on text that the user must *read or operate*. Always fix the floor.

Low contrast, a nonstandard interaction, or restrained motion can be a **deliberate choice** on an expressive surface: a moody hero, a faint watermark, decorative ghost text, an art-directed splash. There, do not fix it automatically. **Flag it as a tradeoff and ask.** For example: "This hero is 2.8:1, below AA. Is that intentional for the mood, or should I lift it?" Decide by role. Does the user *use* the element, or *experience* it? Fix the floor. Flag the expressive choice.

## The critique pass

Do not only list checklist misses. Read the work a second time as a **skeptical senior reviewer**. Write down **every real criticism, each with a location**: `file:line` for code, screen and element for UI. "The table header and body are both 14px/500, so the header disappears" counts. "Could be more polished" does not. A vague criticism changes nothing.

The mode sets the size of this pass. It does not loop back on itself:

- **Audit mode:** the located criticisms *are* the report. No fixes and no second pass.
- **Execute mode:** one pass only. Critique, fix what you found, then verify (inspect again, or run). No forced repeat. This audits code that presumably works. It is not a first draft, and a second loop does not pay off here.

## Seeing the UI

For `ui` and `ux`, judge the rendered result. Spend tokens as if they are scarce. A screenshot is an image, so it is costly and slow. Use the cheapest tool that answers the question, in this order:

1. **Accessibility-tree or DOM-text snapshot.** Playwright's `ariaSnapshot` or the Chrome DevTools accessibility snapshot. It is plain text, at a fraction of a screenshot's tokens. It answers most questions: hierarchy, labels, states, focus order, and what is on the page.
2. **One targeted screenshot.** Take it only when the question is visual (spacing, contrast, alignment, overflow) and the tree cannot show it. Capture the component, not the full page. Use one width, then a narrow width only when responsiveness is in question.
3. **No renderer available.** Say so, read the markup, and reason about it at 360px and 1440px. Do not pretend that you looked.

Use the MCP that is connected (Chrome DevTools or Playwright). If neither is connected, write a small Playwright screenshot script. Do not take a screenshot of what a text snapshot already told you.

## ui

- Spacing and alignment match the neighboring views. Use the project tokens and scale, not magic numbers.
- Hierarchy: one primary action per view. Size, weight, and color match importance.
- Contrast and legibility hold at real sizes. Every state exists: hover, focus-visible, active, disabled, loading, empty, error.
- Responsive: nothing overflows or gets squeezed at narrow widths.

For a review of the visual design itself (direction, type, color, components), use the h00man `ui` skill.

## ux

- Shortest path: cut steps, fields, and confirmations that do not earn their place.
- Feedback: every action gets an acknowledgment that the user cannot miss.
- Errors are recoverable: say what happened and what to do. Never discard the user's input.
- Defaults do the right thing. There are no dead ends: every empty state and error points somewhere.

## code

- Delete first: dead code, unused exports, commented-out blocks, speculative abstraction.
- Before you keep anything new, dedupe it against existing helpers. Names say what things are, in the project's domain terms.
- Error handling lives at the boundary, not scattered through internals.
- **Depth over fragmentation.** Judge module shape with `references/codebase-design/SKILL.ref.md`. Flag shallow modules (the interface is nearly as complex as the body), apply its deletion test (inline the pass-throughs), and propose deepening refactors that fold fragments behind one small interface.

## security

- Secrets stay out of code, config defaults, logs, error messages, and client bundles.
- Everything that user input reaches is parameterized or encoded: SQL, shell commands, HTML (XSS), file paths, redirects.
- Every endpoint and query has an explicit answer to "who can call this?", including object-level access (can user A fetch user B's record by changing an id?).
- External input is validated at the boundary. Bodies and uploads have size limits. File types are checked.
- No PII in logs, analytics, or URLs. Destructive operations have a guard: a confirmation, a dry run, or a scoped WHERE clause.

State each security finding as an attack or failure scenario in one sentence.

## all (deep)

Run every checklist above. Then run an architecture pass over the scope: find the shallow modules, the leaky seams, and the interfaces you cannot test. Apply the deletion test and propose the top deepening opportunities. Rate each one **Strong**, **Worth exploring**, or **Speculative**. Use Explore subagents to cover the areas in parallel.

## Report format

Give each finding:

- **Severity:** blocker, major, minor, or nit. For security: critical, high, medium, or low.
- **Where:** `file:line` for code, screen and element for UI and UX.
- **What is wrong** and **why it matters**, in one or two sentences.
- **Suggested fix**, in one line.

Order the findings by severity. End with the **top 3 to fix first** and an overall verdict in 2 or 3 sentences. For `security`, give an explicit **go** or **no-go** for shipping. In execute mode, then list what changed, and each finding left report-only with its reason.
