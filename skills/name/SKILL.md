---
name: name
description: Generate and screen brand names for a company, product, or feature, from descriptive to invented, with sound and distinctiveness checks. quick gives 20 names; deep asks questions, then screens 100-plus into an HTML shortlist. Not legal clearance. Runs only when the user types /name.
disable-model-invocation: true
metadata:
  opencode/autoinvoke: false
---

# name

Name things the way naming agencies such as Lexicon, Igor, A Hundred Monkeys, and Catchword do. Generate a wide, varied set of candidates, then screen it hard. The best name is rarely in the first 20. It is often in the second hundred. Run the set through the screens below. Deliver the whole pool and a recommended shortlist, with the reasons, in an interactive HTML artifact. The user filters and curates the shortlist there.

This skill generates and screens names. It does not clear trademarks or register domains. It flags likely risks and tells the user what to check. A serious venture still needs a real trademark search (tmsearch.uspto.gov, the WIPO Global Brand Database) and an attorney. Say so. Never say that a name is "available".

Read `reference.md` in this skill folder only for a deep question: case studies, the full legal doctrine, naming for China and global markets, or the academic sources. It holds background facts. When it and this file differ on what to do, this file wins.

## Pick the mode

Both modes make the same HTML artifact. They differ in how much you ask and how many candidates you make.

- **`deep` (the default).** Ask the user up to 10 questions. Generate 100 or more candidates across all the types. Screen them down to a shortlist of 20 to 30 names.
- **`quick`.** Ask nothing, or one question that blocks the work. Generate about 20 candidates with light screening. Return a shortlist of about 20 names.

Use `quick` or `deep` when the user names it. Otherwise read the request. A short "name my X" in a hurry gets quick. A longer request or a real venture gets deep. When you cannot tell, use deep and say so in one line, for example: "Deep mode asks a few questions first. Say *quick* to skip them." The user can switch at any time.

## Step 1. Get a brief

The brief sets the quality of the names. Generation does not.

**Deep mode.** Ask up to 10 questions in batches. `AskUserQuestion` takes 4 at once, so use 1 to 3 rounds. Ask only what the request leaves open. Stop when you know what it is, the feel, the audience, the competitors, the markets, and the hard limits. Spend the questions on what shapes the name:
- **What it is.** One sentence on what it does and how it differs.
- **Feel.** Playful or serious, premium or accessible, warm or sharp. What should the first hearing feel like?
- **Audience.** Who it is for, and what they call this category today.
- **Competitors.** Get 3 to 5 names. Use a different naming pattern.
- **Architecture.** Does it stand alone or sit under a parent brand? Will it need sub-brands later?
- **Forbidden ground.** Words or themes that must appear, that must not appear, or that competitors own. Ask for founder vetoes.
- **Language.** English only, or across languages? Which cultural markets matter (`reference.md` §6 and §8)?
- **Limits.** One market or global? Must it have the .com, or are .ai, .io, and .co fine? Which trademark classes matter?
- **Appetite.** Safe and clear, or open to a name that divides people? The answer sets how far up the type table you push.

Restate the brief in one sentence before you generate. The user can then correct it at low cost.

**Quick mode.** Do not ask the questions. Generate from what you have. State your assumptions inline. Ask one question only when the work is impossible without it, for example when you do not know what the product is.

## Step 2. Generate broad, across the types

Generate 100 or more candidates in deep mode and about 20 in quick mode. Spread them across the name types, so that the user sees real range and not many forms of one idea. Do not settle on the pattern that the category already uses. That is the common failure of lazy naming and of AI name generators. In deep mode, cover every type in the table. A type with no candidates means that you did not generate enough. The template's coverage bar shows that zero in red. In quick mode, cover a broad sample of types. You need not fill each one.

Set each candidate's `type` to the bold label before any slash, for example `Suggestive`, `Invented`, or `Altered spelling`. The template checks these exact labels.

| Type | What it is | Examples |
|---|---|---|
| **Descriptive** | Says what it does | PayPal, The Home Depot |
| **Suggestive / evocative** | Hints at a quality; needs a small leap | Amazon, Slack, Patagonia |
| **Associative / metaphoric** | Borrows images from another domain | Nike, Starbucks, Jaguar |
| **Synonym-driven** | A near-synonym or nearby word for the core idea, used as is | Stripe, Notion, Ramp, Vise |
| **Invented / coined** | A built letter string with no prior meaning | Kodak, Xerox, Häagen-Dazs |
| **Abstract / arbitrary** | A real word with an unrelated use | Apple, Camel, Shell |
| **Compound** | Two whole real words joined | Facebook, Snowflake, Salesforce |
| **Portmanteau / blend** | Two words cut and merged | Pinterest, Instagram, Groupon, Verizon |
| **Acronym / initialism** | Initial letters, said as a word or letter by letter | IBM, KFC, NASA, IKEA |
| **Lexical / real-word** | A plain dictionary word | Oracle, Visa, Dove |
| **Foreign / borrowed** | A word from another language | Volvo (Latin "I roll"), Audi (Latin "listen"), Hulu (Mandarin "gourd") |
| **Palindrome** | Reads the same backward | Sonos, Otto, Aviva, Civic |
| **Reduplicative / rhyming** | Repeated or rhyming syllables | Google, TikTok, Kit-Kat, Yoyo, Juju |
| **Mimetic / onomatopoeic** | The sound is part of the meaning | Twitter, Bing, Schweppes |
| **Altered spelling** | A known word spelled a new way | Lyft, Flickr, Reddit |
| **Affixed** | A root with a common prefix or suffix | Calendly, Grammarly, Shopify |

Use these methods for range:
- **Morpheme tables.** Prefixes such as `inter-`, `trans-`, `ultra-`, `neo-`. Suffixes such as `-ify`, `-ly`, `-io`, `-ai`, `-ex`, `-on`, `-ium`, `-os`.
- **Synonyms.** Find synonyms of the core idea in a thesaurus, then synonyms of those.
- **Roots.** Use foreign words and Latin or Greek roots. Trace the origin and the related forms of each root word.
- **Metaphors.** Ask what it would be if it were an animal, a place, a sound, or a myth.
- **Acronyms.** Write the descriptive phrase. Look in its initials for a string that people can say.
- **Sound patterns.** Use CVCV, alliteration, palindromes, and repetition.

Lead with suggestive, arbitrary, invented, portmanteau, or palindrome names. Avoid purely descriptive names unless the user insists. They are weak in law and easy to forget, and the ones that succeed often rebrand later (KFC, RH, WW). Acronyms are usually weak as the main name. They carry no image. When buyers read the letters as the descriptive phrase, the acronym is as weak in law as the phrase. An acronym that people can say (IKEA, NASA) is easier to say and remember, but it is not stronger in law. Generate a few acronyms. Do not lead with them.

## Step 3. Screen

**Remove duplicates first.** Normalize each name: lowercase, with no spaces, hyphens, or punctuation. Drop exact repeats. Collapse spelling variants and sound-alikes of one root (Lumen, Lumin, Lumena) to the best one. If the unique count falls under the mode's target, generate more before you screen.

Then run the candidates through the screens below. The first pass removes the clear losers. Give each survivor a `score` in Step 4.

**Sound symbolism.** Design the sound as well as the meaning. Listeners link sounds to qualities without effort. These links are tendencies, about 60:40 to 70:30 in published tests. They are not rules. Use them to match the feel to the product. Never reject a name on sound alone. Judge the spoken sounds, not the letters: Apple starts with a front vowel.

- **Front vowels** (i, ee) feel small, light, fast, sharp, friendly, and feminine. **Back vowels** (a, o, u) feel large, heavy, strong, and serious.
- **Fricatives** (f, s, v, z) feel soft, light, and fast. **Stops** (b, d, g, p, t, k) feel hard, crisp, and full of energy.
- Match the sound to the product. Front vowels and fricatives suit light, elegant, or fast things, such as a razor or a fintech app. Back vowels and stops suit heavy, strong, or durable things, such as an SUV or a security platform.
- Lexicon's rules of thumb, not research findings: `v` is lively, `b` is reliable, `z` grabs attention, `x` is innovation, `s` is atmospheric.
- **Bouba and kiki.** Round vowels with voiced lip sounds feel soft (Google, Volvo). Sharp consonants with front vowels feel spiky (Kit-Kat, Tic Tac). Match the feel to the brand.

**Form.**
- **Syllables.** Use 1 to 4. Two is the best count. With more than 4, people shorten the name for you (Federal Express became FedEx).
- **Easy to say and spell on first contact.** If you must explain how to say it (Touareg, Abrdn), it fails. People prefer and remember names that are easy to say.
- **Euphony.** Even consonant and vowel turns (CVCV: Lego, Toyota, Yoda), alliteration (Coca-Cola, PayPal), and palindromes (Sonos, Otto) read well.

**Distinctiveness.** The Abercrombie spectrum is the legal base. Prefer a higher tier, because distinctiveness and legal strength rise together. Treat the tier as a first assessment for specific goods under US law. It is not a legal opinion.

| Tier | Protectable? | Examples |
|---|---|---|
| Generic | Never | "Aspirin", "Escalator" |
| Descriptive | Only with proof of acquired distinctiveness (secondary meaning) | American Airlines, Holiday Inn |
| Suggestive | Yes, inherently | Coppertone, Microsoft, Greyhound |
| Arbitrary | Yes, inherently | Apple, Amazon, Camel |
| Fanciful | Strongest | Kodak, Exxon, Verizon |

**Criteria.** Use Neumeier's 7 as a filter, not as a source of names: distinctive, brief, appropriate, easy to spell and say, likable, extendable, protectable. Watkins' **SMILE** test asks for Suggestive, Memorable, Imagery, Legs, and Emotional. Avoid his **SCRATCH** faults: Spelling-challenged, Copycat, Random, Annoying, Tame, Curse-of-knowledge, and Hard-to-pronounce.

**Knockout search.** Do the search yourself. Do not only advise it. Before the handoff, search every top pick, and each shortlist name that you doubt, with the web search tool. Run the searches in parallel. For each name, search three things: the exact name with the category ("Vellum design app"), the bare name as a company or product, and the closest spelling or sound variant. When a tool can reach tmsearch.uspto.gov or the WIPO Global Brand Database, search there too. Classes do not fully separate marks. A mark for related goods in another class can still conflict. Write what you searched, the date, and what you found in the candidate's `check` field, for example "web + USPTO, 2026-10-27: live mark VELLUM in fonts, class 9". Leave `check` empty for names that you did not search. The artifact shows them as "not searched yet". A clean search is still not clearance. Never call a name "available".

**Risk flags.** Flag these risks. Do not clear them.
- A conflict that your search found, or one that you know of. Put it in `flag`.
- A bad meaning in another language, or a bad word inside the name. This matters most for global names (`reference.md` §6 and §8).
- Genericide risk when the name is too close to the category noun.
- The domain. An exact .com is good to have, not a need. `.ai`, `.io`, `.co`, or a `get-` or `try-` prefix is fine for most ventures. Do not reject a strong name for its domain.

## Step 4. Shortlist with reasons

Return a shortlist of 20 to 30 survivors in deep mode, and about 20 in quick mode. Group them by type to show the range. Do not let one type take over. Each survivor has seven fields, and a flag and a search note when needed:

> **Vellum** · Lexical · *Old translucent drawing surface for sketches and text, tactile and premium.* · sound: velvet-soft, calm · 2 syl · Arbitrary tier · fit 4/5 · flag: a Sketch font is named Vellum, so check related software classes.

**Score** each survivor 1 to 5 for fit to the brief. Keep legal and language risks in `flag`. A risk blocks a name. It does not lower the score.
- **5.** You would lead with it. It stands out in the category, fits the feel, is easy to say and spell, and extends to sub-brands.
- **4.** Strong, with one clear weakness.
- **3.** It works. It is on the list for range.
- **2.** A weak fit. Keep it only because the user may like the angle.
- **1.** Do not shortlist it. Leave it in the pool.

Then pick your top 3 to 5 and give them `rank` 1 to 5. Write one sentence on why each is strongest against the brief. Put it in the `gloss` and repeat it in your chat reply.

**Reactions.** The right name often divides people at first. Andy Grove said of "Pentium" that the split showed energy. Love from everyone on first hearing often marks the safe, typical choice. Find out *why* a name divides people. Unfamiliarity is fine. A wrong way to say it, a bad meaning, or a weak fit is a real objection.

## Output

Always make an HTML artifact, in both modes. It is a triage tool that the user uses to compare, filter, and curate. It is not a report. Start from `template.html` in this skill folder. Do not build it again from a blank page.

Read `references/artifact.md` before you write the artifact. It gives the layout, how to fill the template data, how to change the theme, where to save the file, and the headless checks to run before the handoff.

## Rebrand or keep

When the user asks whether to *change* a current name, read `references/rebrand.md` and apply its tests in order.

## Sources

- h00man, by h00man23 (https://github.com/h00mankind/skills, MIT): this skill is the earlier `h00man:name`, with the artifact steps moved to `references/artifact.md`.
