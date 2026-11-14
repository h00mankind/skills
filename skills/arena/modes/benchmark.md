# benchmark mode

A benchmark is fair only when every part is blind. Give each contender the same prompt. Run each contender alone. Keep the judge blind to who made what. Report only the numbers that you measured.

## 1. Set up (quick by default)

Find the target in the argument: `model` or `skill`. Find the mode: `deep`, or quick when the argument does not say `deep`. Ask only for what is missing.

- **No target.** Ask one question: compare models or skills?
- **Model, quick.** Use the contenders that the user names. If the user names none, use the session's model and one good rival. Use one task.
- **Model, deep.** Ask how many contenders and which. Use 2 or 3 tasks.
- **Skill, quick.** Use one skill, one task, and two conditions on the same model: with the skill and bare.
- **Skill, deep.** Use several skills, or put two skills against each other on the same task.

A contender can be a Claude model or an external CLI agent. A Claude model is fable, opus, or sonnet at any effort (low, medium, high, xhigh, max). Haiku runs only at its default effort, because Haiku 4.5 rejects an effort setting. An external CLI agent is codex, gemini, cursor-agent, or a similar tool. Section 3 says how each kind runs.

In quick mode, ask one question at most. Pick a task yourself, state it, and let the user reject it.

## 2. Pick a calibrated task

Use the user's own task word for word when they give one. Otherwise pick a task of the right size. A subagent must finish it in about 2 to 5 minutes and a few thousand output tokens. Do not pick a trivial task such as fizzbuzz: every contender passes it, so it shows no difference. Do not pick "build an app": it is slow and costly, and the judge cannot score it clearly. Suggest 2 or 3 tasks that match the target:

- **UI.** A pricing card, a dashboard stat widget, or a signup form, as one self-contained HTML file.
- **Copy.** A landing hero and three feature blurbs for a stated product.
- **Code.** A pure function with tricky edge cases, and its tests.
- **Reasoning.** Debug a short broken snippet and explain the root cause.

For a skill, pick a task inside that skill's scope. Test `motion` on an animation brief, not on a SQL query.

## 3. Run parallel subagents, each one alone

Make the run folder `benchmarks/NNNN-<slug>/`. `NNNN` is the next number after the highest run, with leading zeros, for example `0007-opus-vs-sonnet-ui/`. Make `benchmarks/` when it does not exist. Git ignores it, so runs stay local. Put task prompts in `prompts/` and helper scripts in `scripts/`. Put contender artifacts and the report at the root of the run folder. Put no loose files in `benchmarks/`.

Start one subagent per cell (model and task, or condition and task). Start all of them in one message, so that they run at the same time.

- **Same prompt.** Give every contender in a comparison the same prompt. Only the model, the effort, or the added skill text can differ. For a skill condition, put the full SKILL.md in the prompt, because subagents do not inherit skills.
- **Claude contender at an effort.** The Agent tool's `model` override cannot set an effort. Write a temporary agent file first: `.claude/agents/bench-<model>-<effort>.md`. Give it the frontmatter `name`, `description`, `model: <model>`, `effort: <level>`, and `tools: Write, Read, Bash`. Give it a one-line body: "Complete the task exactly as given." Start the cell with that agent type. Delete the `bench-*` files when the run ends. If the session does not load the new file, use the plain model override. Record the effort that applied. Never label a run with an effort that you did not verify.
- **External CLI contender.** Check it first with `command -v`. If it is missing, say so and drop it. Do not put another contender in its place. Run it headless with Bash in the background, with the same prompt from a file, for example `codex exec "$(cat prompts/task.md)"`, `gemini -p "$(cat prompts/task.md)"`, or `cursor-agent -p "$(cat prompts/task.md)"`. Read its `--help` to find the non-interactive flag and how to set a model. Run each one in its own subfolder, so that outputs cannot collide. Give it a wall-clock timeout of about 3 times the task budget. Take tokens and cost from its output when it prints them. Otherwise write `n/a`. The judge scores its artifact the same way as any other.
- **Separate files.** Each agent writes its own file in the run folder (`a.html`, `b.html`, and so on). Never tell an agent that it is in a benchmark or who it competes with.
- **Stats.** Write each run to `stats.json`: the exact model ID, the effort, the condition, the wall-clock time, the output tokens, the input and cache-read tokens, the steps (tool uses), the cost, the artifact size, and the run date. Use the values that the harness reports. Write `n/a` for each value that it does not report. Never estimate a value without saying so. When the harness reports no cost, compute it from the token counts and the model's current prices (see the claude-api reference) and mark it *computed*. When you do not know the prices, write `n/a`.

## 4. Judge blind and score in percent

Give a new subagent the outputs, labeled A, B, C in random order, and the original task prompt. Give it 3 or 4 criteria that fit the task, each scored 1 to 10. The judge scores, picks a winner, and explains why. It does not know which model or condition made which output. Show the mapping only after the verdict.

The headline **score** is a percentage: the points earned divided by the points possible, times 100. For example, 34/40 is 85.0%. In deep mode, average across tasks, with each task at equal weight. Give one number per contender. Compare scores only across runs with the same task set. When the criteria or tasks differ from an earlier run, say so.

## 5. Report with the leaderboard first

End every run with `<slug>-report.html` in the run folder, in quick mode too. The slug in the file name keeps browser tabs apart across runs. Use this order:

1. **The task brief.** Show the exact prompt that the contenders got, so that the reader judges the work against the real request.
2. **The work.** Show UI side by side in iframes, copy as rendered prose, and code with highlighting. For a UI or site artifact, put an `Open full preview` link, styled as a button, above each iframe. Point it at the artifact with `target="_blank"` and `rel="noopener"`.
3. **The leaderboard table.** Rank the contenders by score, one row each: rank, model (with condition or effort), **Score %**, **Cost / task**, **Tokens / task** (output), **Steps / task** (tool uses), wall-clock time, and a winner badge. In deep mode, the per-task numbers are averages. Align numbers right with `tabular-nums`. Put the exact model IDs, the run date, and the harness in the header. Put the judge's scores per criterion in a second table. Use no charts.
4. **The write-up.** Give the judge's verdict with the names shown, your own observations, and a note on sample size. A quick run is one task and one attempt. It indicates a result. It does not prove one.

Open the report with `open <slug>-report.html`. End with an offer of the next step: a deep run, a rematch on another task, or a new contender.
