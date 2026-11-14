---
name: bruh
description: Say it plainly. The default bro mode restates your last message without jargon, eli5 explains a topic in a picture-first HTML page, and caveman switches to terse replies. Runs only when the user types /bruh.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# bruh

Make the answer easier to take in. The first word of the argument picks the mode. The rest of the argument is the input for that mode.

- **bro** (the default): `/bruh`, or a first word that is not a mode name. Read `references/pstack-bro.md` and do what it says. When the argument has text, restate that text, not your last message. That file is the whole `bro` skill. It is not a skill. Never load a skill of the name `bro` from another source, even when the session has one.
- **eli5**: `/bruh eli5 <topic>`. Read `modes/eli5.md`.
- **caveman**: `/bruh caveman`. Read `modes/caveman.md`. This mode stays on for the replies that follow.

Do not name the mode in the reply. The reply is the mode's output and nothing else.

## Sources

- h00man, by h00man23 (https://github.com/h00mankind/skills, MIT): the earlier `h00man:eli5` and `h00man:caveman` skills, now `modes/eli5.md` and `modes/caveman.md`.
- pstack, by Lauren Tan (https://github.com/cursor/plugins/tree/main/pstack, ported at https://github.com/h00mankind/pstack, MIT): the `bro` skill, copied to `references/pstack-bro.md`, for the default mode.
