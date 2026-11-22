---
name: motion
description: Add, tune, review, or remove interface motion. Covers animation, transitions, enter and exit, staggers, springs, drag, swipe, sheets, gestures, scroll effects, and janky frames, on the web and in React Native or Expo. Use when the user asks to animate something, "make it feel alive", fix motion that "feels slow" or "drops frames", build a drag or swipe, audit the animations, or name a motion effect.
---

# motion

Use motion to explain. It shows where a thing came from, that an action took effect, and where to look next. Cut an animation that explains nothing.

## Pick the job

Each folder under `references/` is an upstream skill copied verbatim. Read its SKILL.md first, then the files it links.

- **Build one animation.** `references/animate/SKILL.ref.md`, then its `RECIPES.md`.
- **React Native or Expo.** `references/animate-expo/SKILL.ref.md`.
- **Several things move together.** `references/choreograph/SKILL.ref.md` for stagger, order, and overlap.
- **Drag, swipe, flick, sheet, swipe back, scrubber.** `references/fluid-interactions/SKILL.ref.md` and its `code/` files. Read `references/gestures/SKILL.ref.md` for the feel checklist.
- **Jank or dropped frames.** `references/animation-performance/SKILL.ref.md`.
- **The user describes an effect but has no name for it.** `references/animation-vocabulary/SKILL.ref.md`.
- **"What could animate here?"** `references/find-animation-opportunities/SKILL.ref.md`. It is read-only.
- **"Too much motion" or "feels slow".** `references/remove-animations/SKILL.ref.md`. It only subtracts.
- **Audit a whole codebase.** `references/improve-animations/SKILL.ref.md`. It writes plans, not code.
- **Review a diff or a component.** `references/review-animations/SKILL.ref.md` and its `STANDARDS.md`.
- **Project uses @fluid.** `references/fluid-custom-motion.md` before you write any motion by hand.

## Should it animate at all?

Decide by frequency. A thing seen 100 or more times a day, such as a keyboard shortcut or a command palette, gets no animation. Repeated motion makes the app feel slow. A thing seen tens of times a day, such as a hover or a list move, gets little or no motion. An occasional thing, such as a modal, drawer, or toast, gets standard motion. A rare or first-time thing, such as onboarding or a celebration, can get delight. Do not animate an action that the keyboard starts.

Match motion to the component. A playful thing can bounce a little. A professional dashboard stays crisp and fast.

## Defaults

- **Duration.** Use 100 to 160ms for button feedback, 125 to 200ms for tooltips, 150 to 250ms for dropdowns, and 200 to 400ms for modals and drawers. Keep UI motion under 300ms where you can. A 180ms dropdown feels faster than a 400ms one.
- **Easing.** Use ease-out for enter and exit, ease-in-out for moves and morphs on screen, and plain ease for hover and color. Use linear only for continuous motion, such as spinners. Do not use ease-in for UI, because it delays the start, when the user is watching.
- **Custom curves.** The CSS built-ins are weak. Take the strong ease-out, strong ease-in-out, and iOS drawer curves from the easing tokens in `references/animate/SKILL.ref.md`.
- **Exits are faster.** Use about 60 to 70% of the enter duration. Go slow where the user decides. Go fast where the system responds.
- **Distance is small.** Use 4 to 12px for small moves. Use larger travel only when the element comes from a real place. Start an enter at `scale(0.95)` with `opacity: 0`, never at `scale(0)`.

## Small details

- A pressable gets `:active { transform: scale(0.97) }` with about 150ms of ease-out.
- Popovers, dropdowns, and tooltips grow from their trigger. Set `transform-origin` to the anchor. Radix and Base UI expose it as a CSS variable. Modals stay centered.
- Delay the first tooltip. Open the next ones at once while one shows.
- For anything the user triggers fast, use transitions, not keyframes. A transition retargets while it runs. A keyframe restarts from zero.
- Use `@starting-style` for entry without JS. Fall back to a mounted attribute.
- For off-screen positions, use `translateY(100%)`, not fixed pixels.
- If a crossfade does not blend, add `filter: blur(2px)` during the change.
- Turn transitions off while the theme changes. Add a class such as `[data-theme-switching] * { transition: none !important }` for one frame. Otherwise each element changes color at its own speed.
- A stagger must not block input. The user can click an item before the last item arrives.
- Motion that plays by itself for more than 5 seconds next to other content gets a pause, stop, or hide control. Muted decorative loops stop under `prefers-reduced-motion`.
- For SVG, put the transform on a `<g>` wrapper with `transform-box: fill-box`. Safari places the origin wrongly on other SVG elements. Set `transform-origin` to the real pivot, `center` only when the shape turns around its center.

## Springs and gestures

Springs settle by physics, not by a duration. They keep velocity when interrupted. Use them for drag, gestures that can be interrupted, and motion that tracks the mouse.

For anything the user drags, flicks, or swipes on the web, follow `references/fluid-interactions/SKILL.ref.md`. It covers release velocity, projection, rubber banding, direction lock, and the pointer-event bugs that freeze gestures on phones.

Pick the spring numbers by project:

- **Project has @fluid (Fluid Functionalism).** Use the `spring.fast`, `spring.moderate`, and `spring.slow` tokens from `references/fluid-custom-motion.md` for each state change that the system starts: hover, focus, open and close, enter and exit. Use `springFromResponse` from `references/fluid-interactions/code/physics.ts` only where a finger releases something and the motion must keep its velocity. Do not rebuild an @fluid component that already drags (sidebar resize, queued stack, slider) with fluid-interactions code.
- **No @fluid, gesture work.** Use the springs and defaults in `references/fluid-interactions/references/springs.md`.
- **No @fluid, no gesture.** Use `{ type: "spring", duration: 0.5, bounce: 0.2 }`. It is the easiest to reason about. Keep bounce at 0.3 or less, and skip bounce for most UI.

For React Native, follow `animate-expo` for springs and gestures. Do not use the web gesture code there.

## Performance

- Animate `transform` and `opacity` only. Do not animate `top`, `left`, `width`, `height`, or `margin`. Change size with `scale`, `clip-path`, or FLIP.
- Use CSS for motion you know in advance. CSS runs off the main thread. Use JS for dynamic or interruptible motion. Use WAAPI (`element.animate`) for JS control at CSS speed.
- In Motion, the shorthand `x`, `y`, and `scale` props are not hardware-accelerated. Pass a full `transform` string when smoothness under load matters.
- Do not drive per-frame values through a CSS variable on a parent. Set `transform` on the element.
- Use `will-change` rarely, and remove it when the animation ends. Keep blur at 20px or less.

For a full diagnosis, use `animation-performance`.

## Reduced motion

Under `prefers-reduced-motion`, use fewer and softer motions. Do not remove all motion. Opacity and color feedback stay. Remove movement in space. Set it once at the system level, with a CSS media query or `useReducedMotion`, not per animation. Soften motion that plays by itself, not motion that the finger drives. Put hover effects behind `@media (hover: hover) and (pointer: fine)` so taps do not stick on touch screens.

## Verify

Watch it run at full speed. Then watch it slowed down in the devtools Animations panel, or frame by frame. Timing flaws are hard to see at full speed. Try to break it: trigger it many times fast, interrupt it mid-way, navigate away. Test gestures on a real phone. If it still feels wrong, look again the next day.

## Upstream names in these files

- `animate`, `animate-expo`, `choreograph`, `gestures`, `animation-performance`, `animation-vocabulary`, `find-animation-opportunities`, `remove-animations`, `improve-animations`, and `review-animations` mean the folders of the same name under `references/`.
- `improve-animations` writes plans for other agents. Hand each plan to a subagent, or run it yourself.
- `pick-ui-library` is a reference in the ui skill. If ui is not installed, use `motion` (motion.dev) for springs, layout, and exit animations, and plain CSS for the rest.
- `animation-accessibility` (named in `gestures`): not in the stack. Follow the Reduced motion section above and `references/fluid-interactions/references/reduced-motion.md`.
- `debug-animation` (named in `gestures`): not in the stack. Follow the Verify section above. Record the gesture and step through it frame by frame.
- Each emil file starts with an "Initial Response" section. Ignore it. Do the task.

Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference links to a `SKILL.md`, read the `SKILL.ref.md` in that folder.

## Sources

- **h00man**: `h00man:motion`, h00mankind's earlier motion skill, https://github.com/h00mankind/skills, MIT. Takes the frequency rule, the defaults, the details, the spring rule, and the verify steps.
- **emil**: `animate`, `animate-expo`, `choreograph`, `gestures`, `animation-performance`, `animation-vocabulary`, `find-animation-opportunities`, `remove-animations`, `improve-animations`, and `review-animations` by Emil Kowalski, https://github.com/emilkowalski/skill, MIT. Takes the animation workflows, recipes, audits, and review standards.
- **rndrealm**: `fluid-interactions` by Seun Oyediran, https://github.com/rndrealm/fluid-interactions, MIT. Takes gesture physics, springs by response, and the drag code.
- **mickadesign**: `fluid-functionalism` by Micka Touillaud, https://github.com/mickadesign/fluid-functionalism, MIT. Takes the @fluid spring tokens in `references/fluid-custom-motion.md`.
- **vercel**: the Web Interface Guidelines by Rauno Freiberg and Vercel, https://github.com/vercel-labs/web-interface-guidelines, MIT. Takes the autoplay and SVG transform rules.
