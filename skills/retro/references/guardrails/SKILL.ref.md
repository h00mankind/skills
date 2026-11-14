---
name: guardrails
description: Stop agents from repeating known mistakes in a project, new or old, by making each mistake fail automatically, and keep judgment calls in TASTE.md. Use for /guardrails, when the user corrects the same mistake twice, or when starting a new repo.
---

# Guardrails

An agent sees only the files it opened, copies the nearest example, and takes the shortest path that passes. A written rule it never reads does not stop it. Make each known mistake impossible or make it fail on its own. Keep text only for taste.

This skill names the problems and the bar a fix must clear. You choose the solution. Use the best approach the project's stack and today's tools allow. Prefer what the project already has, and add a tool only when it is clearly the best fit.

## The bar for levels 1 to 4

- **It fails without a reader.** The mistake fails a build, a check, or a test, or the structure makes it impossible to write. An agent that never opens `GUARDRAILS.md` still cannot ship the mistake.
- **It names the way out.** A failure tells the agent what to use instead.
- **It is proved.** It fails on a real past mistake, or on a reproduction of one, and passes on the fix. Unproved means not done.
- **It runs the same everywhere.** The same command locally and in CI.
- **It lets old debt pass.** Existing violations do not block new work, but new ones fail.
- **It gives exceptions a reason.** An exception sits next to the code it excuses, with a reason.

Choose the highest level that works. Each level down is easier to skip.

1. **Structure.** One owner and one supported way. A default that is already right, so nothing has to be remembered. No second way left to copy.
2. **Types.** The wrong value cannot be written.
3. **Automated check.** Lint, static analysis, or a script.
4. **Test.** Behavior the user can see, including behavior at run time.
5. **`TASTE.md`.** Only for a judgment call that no check can decide.

A mechanical mistake (a banned pattern, import, value, or file location) never stops at level 5.

## Steps

1. **Load state.** Read `GUARDRAILS.md` if it exists. Its `Last audit` line gives the commit and date to start from. With no `GUARDRAILS.md`, start 90 days back, or from the first commit in a young repo.
2. **Read the packs** that match the project. A pack lists known problems from earlier projects. For any UI, read `references/ui.md`. If the `emil` plugin is installed, read its problem lists: `improve-animations/AUDIT.md` and `review-animations/STANDARDS.md` when the UI has motion, and `break-ui/CATALOG.md` for layouts that break under worst-case data. Use them for their problems and their bar, not as solutions you must follow. Skip them when `emil` is not installed. Other domains have no pack yet.
3. **Collect evidence.** Find the times the user corrected agent work. Quote the user's words. Give each large source to a subagent that returns only the corrections. This step is done when every source is read and every correction belongs to a kind of mistake.
   - Agent sessions for this project: Claude Code logs under `~/.claude/projects/`, Codex logs under `~/.codex/sessions/`, and any other agent the user runs.
   - Commits: fixes, reverts, and lines changed twice.
   - Review comments on merged PRs, and notes from any feedback tool the project uses.
   - Rules already written in `GUARDRAILS.md`, `TASTE.md`, `AGENTS.md`, and `CLAUDE.md`.
4. **Decide what to guard.**
   - A kind of mistake with two pieces of evidence in this project gets guarded now.
   - A problem from a pack gets guarded now if the project has the surface it applies to. In a new repo, build the structure so the problem cannot start. Do not wait for it to happen first.
   - A kind with one piece of evidence goes under Candidates.
   - A rule in `AGENTS.md` or `CLAUDE.md` that nothing enforces and that was broken again is a repeat. Move it up a level in this run. `TASTE.md` rules follow step 6.
5. **Fix each one** at the highest level that clears the bar. In a new repo, there is no past mistake to prove a check on, so write the mistake on purpose, watch the check fail, and remove it.
6. **Record it.**
   - In `GUARDRAILS.md`, keep the `Last audit` line (commit and date), one row per rule (the rule, what enforces it, and how it was proved), and the Candidates list (each with the user's words, date, and source).
   - In `TASTE.md`, keep one entry per judgment rule: the rule as one sentence that says what to do, the user's words, a real example of right and wrong from the project, and how often it was broken after it was written.
   - Move a `TASTE.md` rule that was broken twice after it was written up a level. Delete a rule once its mistake can no longer happen.
   - Make sure the agent instruction file (`AGENTS.md`, or `CLAUDE.md` if there is no `AGENTS.md`) points to both files and says when to read `TASTE.md`.

## Reply

Lead with the number of problems found and how many now fail on their own. Then, for each problem, give the evidence (the user's words and the count, or the pack), the solution you chose and its level, why a higher level did not work, and how you proved it. Then list the new `TASTE.md` rules and the Candidates. Last, name any domain whose evidence suggests a new pack.
