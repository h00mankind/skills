---
name: ship
description: Get changes out with reviewer-ready discipline. Runs commit, push, PR, review, babysit, and merge as one loop until the PR lands. Also fixes failing CI, resolves merge conflicts, and rebases or takes over an old PR. Use when the user says ship it, commit this, open a PR, babysit, fix CI, full send, full stop, or sol loop. Skip read-only inspection.
---

# ship

Know the diff, prove it works, publish only the intended change, and leave a clean tree.

## Pick the mode

- **Ship** (default): run the ship loop below, from the diff to a merged PR. If the user names a narrower step, such as "commit this", "push it", or "open a PR", stop after that step. "Ship it" runs the whole loop.
- **Babysit**: the PR is already open. Join the ship loop at step 8. Stop at the merge gate and report the PR as ready. Merge only if the user also asked you to merge or ship it.
- **Full send**: the user says "full send". Read `references/tstack-full-send.md`. Run the ship loop under its grant and its testing permission. Its grant replaces the approval rules in Boundaries for this task only.
- **Full stop**: the user says "full stop". Read `references/tstack-full-stop.md`. Run the ship loop, but stop at the merge gate and never merge.
- **Sol loop**: the user says "sol loop" and nothing more. Read `references/tstack-sol-loop.md` and run only its review loop.
- **Modernize**: the user asks to rebase an old PR or to "modernize" it. Read `references/tstack-modernize.md`. Choose MODERNIZE, REWORK, or CLOSE before you push.
- **Takeover**: the user wants you to finish someone else's PR, branch, or agent thread. Read `references/tstack-takeover.md`. Keep the original authors' credit.
- **Tidy**: the user asks to make a PR easy to review or to clean up its commits. Read `references/pstack-make-pr-easy-to-review.md`. Get approval before you rewrite history.

## The ship loop

The loop has one goal: the change is merged into the default branch, every change has proof, and the tree is clean. You own the PR until the goal is met or the stop rule applies. Do not hand back to the user while checks or bots are pending. Wait on the watcher. In T3 Code, the watcher wakes you after you end the turn, so ending the turn to wait is correct there.

1. **Set the scope.** Read `git status`, the branch, and the staged and unstaged diffs. State what belongs to this shipment. Ask before you stage ambiguous changes.
2. **Branch.** Never commit new work to the default branch, usually `main`. Create a branch first.
3. **Verify.** Run the narrowest meaningful test, lint, typecheck, or build. If it cannot run, say why. Never claim a pass you did not see. For proof beyond tests, use the `verification` skill.
4. **Commit.** Stage files by name. Make one cohesive commit per reviewable change. Write an imperative subject under 72 characters, with an optional body for why. Use the `writing` skill for commit text.
5. **Review.** Run the review loop in `references/tstack-sol-loop.md` on the change. Fix the findings you agree with, verify again, and commit the fixes.
6. **Push.** Before the first push, rebase onto the latest default branch. Push to the remote that the branch tracks, with upstream set. A new branch uses the remote of the default branch, usually `origin`. Do not force-push unless the user asks or a mode grants it.
7. **Open the PR.** Read `references/tstack-file-pr.md` and follow it for the duplicate check, the title, the opening, the model line, and filing. Shape the rest of the body with the sections in `references/pr/SKILL.ref.md`: a Summary visual, Evidence, and Merge Danger. Put them after the problem-first opening. Add one line that says how you proved the change. Use the `writing` skill for the prose. Without `gh`, give the compare URL and stop at step 10.
8. **Babysit.** Follow Babysit below. Each round: wait on the watcher, triage, fix, verify the fix, commit, push, and wait again.
9. **Merge.** Merge when the merge gate in `references/tstack-full-send.md` passes on the latest commit. Follow that file's merge rules: the repo's merge method, merge queues, and no `--admin` or other bypass.
10. **Clean up.** Delete temp files, scratch scripts, and logs you made. Remove any worktree you created. Confirm `git status` is clean. If the PR merged in this session, switch to the default branch, pull, and delete the local and remote branch.
11. **Report.** Give the branch, the PR URL, the merge commit or the reason it did not merge, the proof, the review outcome, the decisions you made, and residual risk.

**Stop rule.** Stop the loop after three fix rounds in a row that do not bring the PR closer to the merge gate. Also stop on any blocker in the "Asking the user" section of `references/tstack-full-send.md`. Before you stop, remove the watcher. Then ask the user as that section says, with the harness's question tool (`AskUserQuestion` in Claude Code).

A subagent that opens a PR returns to its parent after step 7. It does not babysit or merge.

## Babysit

1. Read `references/babysit-pr/SKILL.ref.md`. It is the main guide: one watcher per PR, rate limits, reviews, base branch, and merging.
2. Read `references/pstack-babysit.md` for the triage order and its hard rules. Do not use its pacing loop or `gh pr checks --watch`. The watcher from step 1 replaces them.
3. Triage in this order: conflicts, then failing checks, then review comments.
   - Conflicts: follow `references/pstack-fix-merge-conflicts.md`.
   - Failing checks: follow `references/pstack-fix-ci.md`. Fix the root cause in code or tests.
   - Review comments: collect them with `references/pstack-get-pr-comments.md`. Then handle them as the babysit-pr guide says.
4. Post every comment and reply in the format of `references/tstack-leaving-pr-comments.md`.
5. When the merge gate passes, go to step 9 of the ship loop. When the stop rule applies, stop as it says.

## Watching and T3 Code

The tstack files run in T3 Code and fall back outside it. Keep both paths working:

- In T3 Code, use the T3 tools that the files name: `watch_pull_request`, `unwatch_pull_request`, `orchestrator_capabilities`, and `delegate_task`.
- Elsewhere, watch with `references/babysit-pr/scripts/pr-wait`. Call it by its absolute path. In Claude Code, run it with the Monitor tool.
- For sol loop outside T3 Code, run Sol through `codex exec`, per the Routing rules in the Models section of `../build/SKILL.md`. Keep one session for all rounds with `codex exec resume`.
- When no route runs Sol, review with Fable instead (`fable @high`, or `claudeAgent/claude-fable-5-2 @high` in T3). Say so in the report, and write the PR line as `Reviewed with sol-loop: <N> rounds with Fable 5.2 (Sol unavailable). Verdict: approved`. This replaces the file's rule that blocks without Sol.
- Never write your own polling or `sleep` loop.

## Boundaries

- Merge only at step 9 of the ship loop, after the merge gate passes. Never merge in Full stop, or when the user named a narrower step.
- Do not amend, rebase, reset, `git clean`, or delete branches that hold user work without approval. Modernize, the full modes, and the ship loop grant a rebase of the task's own branch and `--force-with-lease`, nothing more.
- Do not rewrite history on a branch that other people may have pulled.
- Do not `git add .` in a dirty tree.
- Do not commit secrets, env files, logs, or unrelated dependency changes.
- Do not add unrelated cleanup to the PR. Keep the PR at the size the goal needs.
- Do not skip hooks with `--no-verify`. Do not make a failing check optional so that CI passes.
- Change a test's expected value only when the behavior changed on purpose.

## Names in the included files

The included files name skills from tstack, pstack, and matt. h00man is standalone, so never load one from another source. Map them like this:

- `/babysit`, the command or Cursor's built-in: Babysit mode in this skill.
- `file-pr`: `references/tstack-file-pr.md`.
- `babysit-pr`: `references/babysit-pr/SKILL.ref.md`. Its pr-wait script is `references/babysit-pr/scripts/pr-wait`.
- `full-send`, including the `../full-send/SKILL.md` link in full-stop: `references/tstack-full-send.md`.
- `sol-loop`: `references/tstack-sol-loop.md`.
- `leaving-pr-comments`: `references/tstack-leaving-pr-comments.md`.
- `modernize`: `references/tstack-modernize.md`.
- **fix-ci**, **fix-merge-conflicts**, and **get-pr-comments**: the `references/pstack-*.md` file of the same name.
- The `poteto-mode` **Babysit** playbook, the "platform mapping" lines, and `bugbot-triage.md` are not part of this skill. Use the babysit-pr guide in their place.
- The Claude Code `loop` skill: do not use it here. Use the watcher.
- `interrogate`: the `build` skill.
- `unslop`: the `writing` skill.
- `GLOSSARY.md` in the pr template: the project's own glossary. Many projects name it `CONTEXT.md`.

Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference links to a `SKILL.md`, read the `SKILL.ref.md` in that folder.

## Sources

- h00man:ship, by h00mankind, https://github.com/h00mankind/skills, MIT. Takes the process, the cleanup and report steps, and the boundaries.
- tstack, by Theo Browne (https://github.com/t3dotgg/tstack), MIT. Takes `file-pr`, `babysit-pr` with its `pr-wait` script, `full-send`, `full-stop`, `sol-loop`, `leaving-pr-comments`, `modernize`, and `takeover`.
- pstack, by Lauren Tan, ported for Claude Code and Codex by h00man (https://github.com/cursor/plugins, folder `pstack`), MIT. Takes `babysit` for the triage order and hard rules, `fix-ci`, `fix-merge-conflicts`, `get-pr-comments`, and `make-pr-easy-to-review`.
- matt, by Matt Pocock (https://github.com/mattpocock/skills), MIT. Takes `pr`, the PR body template. Its Summary visuals come from Dex Horthy's `show-me` skill (Humanlayer), as its `CREDITS.md` says.
