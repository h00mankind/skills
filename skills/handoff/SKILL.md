---
name: handoff
description: Write a handoff file so a fresh agent, another harness, another repo, or a colleague can continue the current work without re-deriving it. Runs only when the user types /handoff.
argument-hint: "What will the next session do?"
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# handoff

Turn this session into one Markdown file that travels. A fresh agent reads it and starts work in its first turn.

Focus of the next session (if empty, continue the current work): $ARGUMENTS

## Reference files

- `references/matt-handoff/SKILL.ref.md`: the upstream skill. Read it once. Take the save location, the rule to point at artifacts and not copy them, and the redaction rule.
- `references/matt-phase-boundaries.md`: when a handoff is the right move. Read it when the session can maybe continue here. Take the order of the five options.

## Before you write

1. Check that a handoff fits. A handoff is for work that leaves this context: a new harness, a new directory or repo, a colleague, or a side task found mid-phase. If the next step stays here, tell the user in one line that continuing or `/compact` loses less. Then write the file anyway, because the user asked.
2. Get the facts from the real artifacts, not from memory. Run `git status`, `git log --oneline -5`, and `git diff --stat` in each repo you touched. Note the branch, the open PR, and the last test or build result with its command.
3. If the focus argument narrows the work, keep only what serves that focus. Drop the rest.

## The file

Save it to `$TMPDIR/handoff-<repo>-<yyyymmdd-hhmm>.md` (else `/tmp`). Never save it in the workspace. Use these sections in this order. Leave out a section that has nothing in it.

- **Goal.** One paragraph: what the work is for and what done looks like.
- **Start here.** The repo path, the branch, and the first command or file to open.
- **State.** Done and verified, in progress, not started. Give each verified item its proof (the command and its result). Name failed tests and skipped steps plainly.
- **Decisions.** Each choice with its reason. Add what you tried and rejected, so the next agent does not try it again.
- **Next steps.** Ordered and concrete. The first step can start without a question.
- **Gotchas.** Facts the next agent would lose time to find again: odd setup, flaky tests, wrong leads.
- **Pointers.** Absolute paths or URLs to plans, specs, issues, PRs, commits, and artifacts. Point to them. Do not copy their content.
- **Suggested skills.** The h00man skills that fit the next steps, each with the reason. Use the names in the map below.
- **Open questions.** Choices only the user can make.

## Rules

- Write for a stranger. Use absolute paths. Define each project term once, or point to the `CONTEXT.md` that defines it.
- Keep the file under 150 lines. Detail that is longer belongs in an artifact that the file points to.
- Redact secrets and personal data: keys, tokens, passwords, emails, and private names. Name the place a secret lives, not its value.
- Do not claim state you did not check in step 2.
- Read the file once more as the next agent. Fix each gap where that agent would have to ask.

## Upstream names

- `/handoff` in the phase boundaries guide is this skill. The bold **why** there is a plain word, not a skill.
- "Suggested skills" in the upstream skill means h00man skills: `build`, `ship`, `ui`, `motion`, `verification`, `writing`, `copy`, `present`, `plan`, and the other skills in this set.

## Reply

Give the absolute path of the file. Then give one line the user can paste to start the next session:

```
Read <absolute path> and continue from "Start here".
```

In T3 Code, start a new thread with that line only when the user asks for one.

## Sources

- matt, by Matt Pocock (https://github.com/mattpocock/skills, MIT): the `handoff` skill, copied to `references/matt-handoff/`, for the save location, the pointer rule, the suggested skills, and redaction. The phase boundaries guide from `ask-matt`, copied to `references/matt-phase-boundaries.md`, for when a handoff fits.
- h00man, by h00mankind (https://github.com/h00mankind/skills, MIT): the retired `h00man:handoff` rewrite, for the section list (goal, state, decisions, next steps, gotchas).
