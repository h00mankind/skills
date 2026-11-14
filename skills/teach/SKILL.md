---
name: teach
description: Teach the user a subject, in explain mode (one plain account of code, a change, or a concept, in chat) or course mode (a learning workspace with a mission, lessons, and records over many sessions). Runs only when the user types /teach.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# teach

Make the user understand something, at their pace. Explain mode gives one plain account in the chat. Course mode keeps a learning workspace on disk, so the user can learn a topic over many sessions.

## Reference files

- `references/pstack-teach.md`: the explain mode. Read it in full before an explain run. Take the five steps, the voice rules, and the reply rule.
- `references/matt-teach/SKILL.ref.md`: the course mode. Read it in full before a course run. Take the workspace layout, the mission, lessons, assets, and the knowledge, skills, and wisdom split.
- `references/matt-teach/MISSION-FORMAT.md`, `RESOURCES-FORMAT.md`, `LEARNING-RECORD-FORMAT.md`, and `GLOSSARY-FORMAT.md`: the format of each workspace file. Read a format file before you write that file.
- `references/pstack-unslop.md`: the catalog of AI patterns. Apply it to every reply, lesson, and reference document.

## Upstream names

h00man is standalone. Read these names in the included files as follows:

- **how**: `../build/references/how/SKILL.ref.md`.
- **why**: `../build/references/why/SKILL.ref.md`.
- Before you run how or why, read the Name map, Paths and agents, and Models sections of `../build/SKILL.md`. Apply them to those files: model roles come from `agent-models.md`, not `pstack-models.md`. Give each how or why subagent the absolute path of `../build/SKILL.md` and tell it to apply the same sections.
- **unslop**: `references/pstack-unslop.md`.
- The platform-mapping links in pstack's teach: `../build/references/poteto-mode/references/codex-tools.md`, `copilot-tools.md`, and `muse-tools.md` in the same folder.
- The image-generation tool: use it when the session has one. Otherwise draw an inline SVG or a mermaid diagram with short labels.
- `/teach` in Matt's file: this skill.

## Pick the mode

The first word of the argument picks the mode. The rest is the subject.

- **explain**: the subject is code, a diff, a subsystem, or one concept the user wants to understand now.
- **course**: the user wants to learn a topic over time, or the current folder already has a `MISSION.md`.

With no mode word, choose from the request. A subject inside this repo runs explain. A general topic to learn runs course. When you cannot tell, run explain and offer a course at the end. Name the mode in your first line.

## Explain mode

1. Read `references/pstack-teach.md`.
2. Decide what the user must leave with. Read their goal and level from the conversation.
3. Read the code to get oriented. Then run how and why in parallel subagents, sized to the question. Keep why narrow unless the reasons are the point.
4. Give the smallest complete answer first. Stop and let the user lead. Add depth when they ask.
5. For three or more moving parts, draw a series of diagrams. Each diagram adds one part to the last.

Write no files in explain mode.

## Course mode

1. Read `references/matt-teach/SKILL.ref.md`. Treat the current folder as the workspace.
2. If `MISSION.md` is missing or empty, interview the user about why they want to learn the topic. Write `MISSION.md` from the answers.
3. Fill `RESOURCES.md` with trusted sources before the first lesson. Search the web. Do not teach from memory alone.
4. Read `./learning-records/` and `NOTES.md`. Pick the next lesson inside the user's zone of proximal development.
5. Read `./assets/`. Build the lesson from the components there. Add a new reusable part to `./assets/`, not inline in the lesson.
6. Write the lesson to `./lessons/` as one short HTML file. Add or update the reference documents and `GLOSSARY.md` that it needs.
7. Open the lesson for the user (`open <file>` on macOS).
8. After the user shows real understanding, write a learning record.

When the course topic is code in this repo, use how and why as the knowledge source, and cite the files and commits in place of web resources.

## Choices only h00man makes

- **Quizzes.** pstack forbids quizzes and Matt depends on them. Ask no quiz questions in the chat, in either mode. Put retrieval practice and quizzes only inside course lessons, where the HTML gives instant feedback.
- **Interview.** pstack reads the user's goal from the conversation. Matt interviews for the mission. Interview only in course mode, and only to fill `MISSION.md`.
- **Sources of truth.** In explain mode, the code and its history are the sources. In course mode, cite `RESOURCES.md`, or the code when the topic is this repo.
- **Voice.** Use pstack's voice rules for chat text and for lesson text in both modes. The user's own writing rules (AGENTS.md, CLAUDE.md) come first.
- **Diagrams.** pstack's build-up rule (one new part per diagram) applies inside lessons too.

## Sources

- pstack, by Lauren Tan (https://github.com/cursor/plugins/tree/main/pstack, ported at https://github.com/h00mankind/pstack, MIT): the `teach` skill, copied to `references/pstack-teach.md`, for explain mode. The `unslop` catalog, copied to `references/pstack-unslop.md`, for the prose rules.
- matt, by Matt Pocock (https://github.com/mattpocock/skills, MIT): the `teach` skill and its four format files, copied to `references/matt-teach/`, for course mode.
