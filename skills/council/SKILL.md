---
name: council
description: Get a review or a second opinion on existing work from several strong models of different providers. Runs only when the user types /council.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# council

Several models of different providers review or advise on work that exists. Council makes no new artifact.

## Steps

Follow `references/tstack-council.md`, with these overrides.

- **Members.** Take them from the user's instructions first, such as the Council line in `agent-models.md` in `${CLAUDE_CONFIG_DIR:-$HOME/.claude}/`. That line sets the number of members and their providers. With no named members, take the newest flagship model of three providers.
- **Routing.** Route each member with the Routing rules in the Models section of `../build/SKILL.md`. A `|` choice on the sheet is allowed, not a substitute. When the user or the sheet names the members, add no model outside them and their `|` choices. When all choices of a member fail, drop the member and report it.
- **Options.** In T3 Code, pass only the options that `orchestrator_capabilities` lists for that model.
- **Scores.** Ask for a score out of 10 in every review, not only a code review.
- **Stop.** Stop when a round gives no finding that you agree with. Report each finding that you reject, with the reason.

## Sources

- tstack, by Theo Browne (https://github.com/t3dotgg/tstack, MIT): the `council` skill, copied to `references/tstack-council.md`.
