# UI pack

Known UI problems that apply to any project with a UI. Each entry says what goes wrong, how a user tends to say it, and how to know it is solved. The solution is yours. The quotes help you find the same problem in session logs.

## Problems

**Surface and edge colors picked by hand.** This happens when the component library has no surface or edge color for the case. The screen then invents its own see-through fills and borders, so panels stop matching and surfaces blend into the background. Said: "blending with bg", "more subtle border", "border is missing", "different surface". Solved when the colors the library lacks are added once to a named set next to the library's own, and a value picked by hand fails. Where the library already has the color, use it.

**Scroll areas that behave differently.** One scroll area clips its content at a hard edge, another shows a native scrollbar, another fades. Said: "don't clip it", "apply this here too". Solved when every scroll area uses the project's one scroll part, so its edge and scrollbar behavior is set in one place and nobody has to remember it.

**Layout shift.** Panels, controls, or content move after the first frame when the user changes pages or opens and closes a panel. Said: "it shifts the layout", "it jumps when I switch pages". It happens at run time, so no static check can see it. Solved when an automated run through the app's routes and toggles fails on any box that moves after its first frame.

**Spacing and alignment drift.** Outer padding, margins, and edges differ by a few pixels between screens. Said: "spacing is not consistent", "align with", "should be the same size". Solved when screens cannot set their own outer spacing. Spacing inside a screen stays a judgment call.

**Mixed icon sources.** Inline SVG, Unicode glyphs, or a second icon pack sit next to the project's icons. Said: "don't use svg or unicode". Solved when icons come from one source and anything else fails.

**Raw form controls.** Native select, color, range, checkbox, or radio inputs appear with browser styling. Said: "fix this" about a native input. Solved when each control kind has one styled part and a raw input fails.

**Corners off the scale.** Radii appear that are not in the design, such as tiny corners or charts with their own radius. Solved when every radius, in CSS and in drawn graphics, comes from a few named roles, and a radius picked by hand fails.

**Text and control sizes off the scale.** Text sizes and control heights are set by hand, so one dropdown or button looks smaller than the rest. Said: "the only one with smaller size". Solved when text uses a fixed set of type roles, controls share one height, and a size set by hand fails.

**Empty panels.** A panel opens with nothing in it. Solved when a panel cannot open without content.

## Taste problems

These need judgment, so they belong in `TASTE.md`. Add one only when the project's own evidence repeats it, and use the user's words from that project.

- **Wrong region.** Controls show up where the work goes, or the work shows up where the controls go. Said: "move this to the sidebar", "should be in the main panel".
- **Too much on screen.** Labels repeat what a control already shows, and details that could live in a tooltip take up space. Said: "no need label", "get rid of this", "show on hover", "redundant buttons and texts".
- **Wrong icon for the job.** An icon that suggests a different action, such as a menu icon on a toggle.
