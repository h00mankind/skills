---
name: writing
description: Write, review, or clean up prose that engineers and agents read. Use for docs, a README, an RFC, an ADR, a guide, commit messages, PR descriptions, a SKILL.md, AGENTS.md or CLAUDE.md, or to unslop, de-AI, or line-edit supplied text. Skip product and marketing copy (copy), verify and driver skills (verification), and media prompts (prompt).
---

# Writing

Write so the reader understands on the first read. Pick the job first, read the reference for that job, then run one cleanup pass.

## Scope

- Docs, READMEs, RFCs, ADRs, guides, and reference pages.
- Commit messages and PR descriptions. The `ship` skill commits and opens the PR. This skill writes the text.
- Documents an agent reads: a skill, `AGENTS.md`, `CLAUDE.md`, or a doc behind a pointer.
- Cleanup of prose that the user supplies.

Send other work away:

- Landing pages, headlines, UI labels, errors, and empty states go to the `copy` skill.
- A one-off prompt for another agent or a later run goes to the `/ambitious` command.
- Image, video, and audio prompts go to the `prompt` skill.

## Order of work

1. **Find the terms.** Read the repo's `CONTEXT.md` or glossary if it has one. Use its terms for each concept. Do not invent a synonym.
2. **Pick the job.** Use the table in the next section. Read the reference that the job names before you write.
3. **Write the draft.** Use the real file, flag, command, and symbol names from the code.
4. **Clean it.** Run the cleanup pass for the job (see "Cleanup").
5. **Check it.** Make each count, path, and command in the text true at the commit that ships it. Run the command if you can.

## Jobs and references

**Human docs (README, RFC, ADR, guide, reference).** Read `references/pstack-technical-writing.md` in full. Take the mode choice (tutorial, how-to, reference, explanation), the sentence rules, and the rules against two readings. Pick one mode per document.

- Save an ADR in `docs/adr/` as one Markdown file. Record the choice, the context, and the options you rejected.
- Save a plan or PRD through the `/plan` command, not this skill.

**Commit messages and PR descriptions.** Read the "Voice and repo specifics" part of `references/pstack-technical-writing.md`. Write the PR body as a short briefing. Open it with the goal of the change. Link long logs and do not paste them.

**Agent-facing docs (skills, AGENTS.md, CLAUDE.md).** Read `references/writing-for-agents/SKILL.ref.md`. Take its terms: context pointer, the two loads, the information hierarchy, completion criteria, leading words, and pruning. For a skill, also read `references/writing-for-agents/SKILL-MECHANICS.md` for the frontmatter and the choice between model and user invocation.

- Front-load the trigger words a user types in the `description`.
- Keep each rule in one place. Point to a reference file and do not copy it.
- Write in the second person, imperative mood, and active voice.

**Cleanup of supplied prose.** Read `references/unslop/SKILL.ref.md` for its contract. Keep the author's facts, claims, and certainty. Return the edited text first.

## Cleanup

`references/pstack-unslop.md` is the one pattern catalog for all text. Its rule numbers are stable, so cite a rule by number in a review or when you explain an edit.

- **Text you write** (docs, commits, PRs, agent docs, replies): apply every rule.
- **Text the user wrote**: apply every rule too, an em dash included. The contract in `references/unslop/SKILL.ref.md` limits the edit. Change no fact, claim, or level of certainty, and keep the source's formality and point of view. Use the pattern list in that file only for a pattern that the catalog does not name.

Remove AI patterns as you draft, because a later pass misses some.

## Upstream names in the references

The references name skills that this skill set does not include. Use these files in their place:

- "the **unslop** skill" or "`unslop`" in `references/pstack-technical-writing.md` means `references/pstack-unslop.md`. Its "abstract-metaphor rule" is rule 26 there. Propose a new word for rule 26 in your reply. Do not edit the file.
- "`writing-for-agents`" and "`SKILL.md`" in `references/writing-for-agents/SKILL-MECHANICS.md` mean `references/writing-for-agents/SKILL.ref.md`.
- `/technical-writing` means this skill.
- "your product's copy guidelines" means the `copy` skill.

## Done

- The document has one mode, and each heading states its point.
- Each term matches the repo glossary.
- No pattern from `references/pstack-unslop.md` remains.
- Each path, count, and command is true.

Each upstream `SKILL.md` in `references/` is named `SKILL.ref.md`, so that no harness loads it as a skill. When a reference links to a `SKILL.md`, read the `SKILL.ref.md` in that folder.

## Sources

- **pstack** by Lauren Tan (poteto), Claude Code port at https://github.com/h00mankind/pstack, MIT. Takes `technical-writing` (the four layers for docs, commits, and PRs) and `unslop` (the numbered catalog of AI patterns for all text).
- **matt** by Matt Pocock, https://github.com/mattpocock/skills, MIT. Takes `writing-for-agents` (context pointers, the two loads, the information hierarchy, and skill mechanics).
- **h00man:unslop** by h00man23, https://github.com/h00mankind/skills, MIT, derived from Lauren Tan's unslop. Takes the line-edit contract for supplied prose, which protects the author's facts and certainty.
