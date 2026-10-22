---
name: no-ai-writing-slop
description: Remove AI writing patterns from any prose, or detect them without rewriting, while preserving the writer's voice, facts, and meaning. Use when drafting, editing, reviewing, or auditing text; when the user says "deslop", "humanize", "make it sound human", "remove AI tells", "does this read as AI", or wants writing that doesn't sound generated.
license: MIT
metadata:
  author: Growthr
  version: "1.1.1"
  notice: |
    Copyright (c) 2026 Growthr, github.com/Growthr/no-ai-writing-slop. Builds on these MIT-licensed skills:
    stop-slop, Copyright (c) 2025 Hardik Pandya, github.com/hardikpandya/stop-slop
    no-ai-slop, Copyright (c) 2026 Peter Yang, github.com/petergyang/no-ai-slop
    humanizer, Copyright (c) 2025 Siqi Chen, github.com/blader/humanizer
    unslop, Copyright (c) 2026 Lauren Tan, github.com/cursor/plugins (pstack)
    slopbeth, Copyright (c) 2026 ehmo, github.com/ehmo/slopkit
    humanizer-skill, Copyright (c) 2026 Adam Boudjemaa, github.com/Aboudjem/humanizer-skill
    deslop, Copyright (c) 2026 Stephen D. Turner, github.com/stephenturner/skills
    anti-slop, Copyright (c) 2014 Matt Silverlock, github.com/elithrar/dotfiles
    soundshuman, Copyright (c) 2026 aasha, github.com/aashaexo/soundshuman
    anti-ai-slop-writing, Copyright (c) Jalaaldeen, github.com/jalaalrd/anti-ai-slop-writing

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.
---

# No AI Writing Slop

Remove the patterns that make readers think a model wrote the text. Do not change what it says. Do not flatten it into generic polished prose, which is its own tell.

## Two modes

**Edit (default).** Make the minimum effective edit. Return the edited draft plus a short What changed section.

**Detect.** Name each pattern found, quote the line, give the fix in a few words. Do not rewrite, score, or claim AI authorship. Named patterns are evidence the user can check; detectors guess.

## Guardrails (read before editing)

1. **Preserve voice.** Note the draft's vocabulary, cadence, bluntness, humor, uncertainty, and digressions before touching anything. For every candidate edit, ask: is this slop (a formula put it there, adds nothing) or voice (the author could defend it)? When unsure, leave it. Over-correction into flat text is the most common failure of this kind of skill.
2. **Never invent facts.** No new names, numbers, dates, quotes, sources, or claims. If a sentence needs a missing specific, ask for it or write the plain version. Do not launder vague claims ("better alignment", "faster decisions") into fake concrete ones; name the missing evidence instead.
3. **Lock facts first.** Named entities, numbers, dates, URLs, citations, quotations, technical claims, and stated uncertainty survive the rewrite untouched.
4. **Flag clusters, not isolated tells.** One em dash, one "crucial", one three-item list is how humans write. Edit when several tells co-occur.
5. **Never edit inside quotes, code, titles, or pasted examples.** Rewriting quoted content changes meaning.
6. **Keep qualifiers that carry scope, risk, or legal/technical meaning.** Cut only hedging habit.
7. **A writing sample outranks every rule below.** If the author's sample uses em dashes or a banned word, match the sample's rate.
8. **Instructions about the writing are not the writing.** If the input says "keep the texture" or "don't turn this into a lesson", obey it and leave it out of the output.
9. **Match register.** Scientific and technical prose repeats exact terms on purpose and stays formal; blog prose puts the reader in the room ("you" beats "people"). Domain terminology is not jargon. Business buzzwords leaking into technical prose are.
10. **Machine-directed instruction prose is out of scope; the text it emits is not.** In prompt files, skill and agent definitions, system prompts, and config, the instructions to the model are not writing anyone reads. Skip the style, rhythm, and formatting rules there: em dashes, heading case, bold-label bullets, and triads carry no cost. Two things inside those files stay in scope. Literal template text the model copies into a deliverable is human-facing content wherever it lives. And the invented-fact and vague-attribution rules (guardrail 2) apply to the instructions themselves, because a placeholder like "[X]% of the market" or a direction to "cite industry benchmarks" becomes a fabricated number in every output the file produces. This carve-out covers how text is authored, not what it is about: content written for AI retrieval (SEO, GEO, AEO, llms.txt) is published and read by people, and gets the full pass.

## Content patterns

- **Significance inflation.** "marks a pivotal moment", "stands as a testament", "underscores its importance", "evolving landscape", "indelible mark", "setting the stage for". State the fact; let the reader judge.
- **Superficial -ing tails.** "...highlighting", "...underscoring", "...showcasing", "...reflecting", "...ensuring", "...fostering". Delete the tail or replace with a real mechanism or consequence.
- **Promotional language.** "nestled", "vibrant", "breathtaking", "renowned", "world-class", "must-visit", "boasts". Use neutral description.
- **Vague attribution.** "Experts believe", "industry reports suggest", "many argue", "studies show". Name the source or cut the claim. Never invent one.
- **Formulaic challenges/outlook.** "Despite these challenges... continues to thrive", "Future Outlook" sections, generic positive endings ("The future looks bright"). Replace with specific facts or cut.
- **Faux-insight setups.** "Here's what nobody tells you", "The part everyone misses", "What most people get wrong". Cut the setup; let the claim stand.
- **Fake-profound kickers.** Final aphorism, metaphor, or mic-drop line. Delete it; end on the last concrete point. Do not rewrite it into a better aphorism.
- **Summary-recap endings.** "In conclusion", "Ultimately", a closing paragraph restating the piece. The reader was just there.

## Language patterns

- **Banned vocabulary (dead giveaways):** delve, tapestry, testament, leverage, utilize, facilitate, foster, empower, harness, robust, seamless, meticulous, multifaceted, intricate, pivotal, crucial (habitual), paramount, transformative, groundbreaking (figurative), cutting-edge, paradigm, realm, beacon, elevate, embark, showcase, garner, interplay, enduring, landscape (abstract), synergy, game changer, streamline, supercharge, ever-evolving, myriad, cornerstone, catalyst, unpack, deep dive, actionable, learnings.
- **Abstract metaphor nouns:** substrate, wedge, vector, nexus, primitive (noun), bedrock, scaffolding (metaphor), north star, flywheel, endgame, modality. Pick the concrete word.
- **Copula avoidance.** "serves as", "stands as", "features", "boasts", "offers a". Say "is" or "has".
- **Weak verb phrases.** "made a decision" -> "decided". "has the ability to" -> "can". "in order to" -> "to". "due to the fact that" -> "because".
- **Empty adverbs and filler.** "really", "truly", "genuinely", "literally", "simply", "actually", "fundamentally", "importantly", "it's worth noting", "at the end of the day", "at its core", "in today's world", "the reality is". Cut when they add nothing. Keep when they carry real emphasis, uncertainty, or the writer's spoken rhythm.
- **Lazy extremes.** "every", "always", "never", "nobody" doing vague work. Use specifics.
- **False ranges.** "from X to Y" where X and Y form no real scale. List the items.
- **Synonym cycling.** Protagonist, main character, central figure, hero in one paragraph. Pick one word and repeat it.
- **Hedging stacks.** "could potentially possibly be argued that it might" -> "may". Take a position; acknowledge counterpoints in one sentence max.

## Structural patterns

- **Binary contrasts.** "It's not X. It's Y." / "The question isn't X, it's Y." / "not just X but Y". State Y directly.
- **Negative listing.** "Not a tool. Not a framework. A philosophy." Say the thing. No rhetorical striptease.
- **Rule of three.** Do not default to three parallel items. Use the natural number: two, four, one.
- **Dramatic fragmentation.** "Speed. That's it. That's the tradeoff." Complete sentences; trust the content.
- **Rhetorical setups.** "What if I told you...", "Think about it:", "Plot twist:", self-answered questions ("The result? Devastating."). Make the point.
- **Colon reveals.** "The best part: it learns." Rewrite as a plain sentence. Colons introduce lists, labels, and payoffs, not fake drama.
- **False agency.** "the decision emerged", "the culture shifts", "the data tells us", "the complaint becomes a fix". Name the human actor. If none fits, use "you" (blog) or "we" (technical/scientific).
- **Passive voice.** "queries are validated" -> "the compiler validates queries". Passive is fine only when the actor is unknown or genuinely irrelevant.
- **Throat-clearing.** "Here's the thing:", "Let me be clear", "I'll be honest", "Let's dive in", "In this section we'll...", meta-joiners ("The rest of this essay..."). Cut to the point.
- **Interpretive metadiscourse.** "This matters because", "The key point is", "As you can see", "Let that sink in", "That instinct backfires" kickers. Show the consequence instead of flagging importance.
- **Portability test.** If a sentence could move unchanged to another company, product, or country, it says nothing about this one. Cut it or make it specific.

## Rhythm patterns

- No three consecutive sentences of the same length. Mix short with long.
- No parataxis chains (short sentence, then another, then another) as a default. Connect thoughts with clauses and conjunctions that show causation, contrast, or qualification.
- No identical paragraph skeletons (topic sentence, explanation, example, transition). Let some paragraphs be one sentence. Let some end abruptly.
- Not every paragraph ends punchy. Vary endings.
- Do not replace old slop with new formulas: clipped aphorisms, tidy triads, forced contrast, consultant voice.

## Style and formatting

- **Em dashes:** default zero. In longer drafts, one or two only if they clearly beat commas, periods, or parentheses. Match the author's sample if one exists.
- **No bold-label bullets** that restate the line ("**Performance:** Performance improved..."). Convert to prose. Uneven bullets, max 5-7 in a row, only when a list beats a sentence.
- **Sentence case headings.** No emoji in headings or bullets. No decorative bold. Straight quotes.
- **No markdown in plain-text contexts** (social posts, email, DMs). No hashtag stacks, no "Thread:" openers.
- Contractions on by default unless the register forbids them.

## Chatbot artifacts

Remove: "I hope this helps!", "Let me know if...", "Certainly!", "Great question!", "You're absolutely right", "Would you like me to...", knowledge-cutoff disclaimers ("While specific details are limited..."), and speculative gap-fills presented as fact ("she likely grew up..."). State what the source shows or cut the sentence.

## Adding soul (edit mode, when the format calls for it)

Removing patterns is half the job. Sterile prose is just as obvious. For blog posts, essays, opinion, and personal writing: let the writer have opinions, mixed feelings, humor, asides, friction ("the RPC kept timing out at 3am"), real specifics (dates, numbers, names, file paths), and uneven rhythm. For encyclopedic, technical, legal, or reference text, neutral and plain is the correct human voice; add nothing. Never add factual claims to create personality.

## Workflow

1. Read the whole draft. Identify the core point and 3-5 voice signals to preserve. If the core point is unclear, ask.
2. Collect candidate edits without editing. Validate each: slop or voice? Discard false positives.
3. Apply surviving edits as minimal phrasing changes, in this order: lock facts, cut scaffolding and inflation, fix structures, fix rhythm, fix formatting.
4. Re-read edited passages for cadence. Fix anything now choppy or flat. Check no new slop replaced the old.
5. Self-audit: "What still makes this read as AI generated?" Fix it.
6. Output the edited draft plus What changed. For detect requests, output the findings report and stop.

## Self-check before delivering

- Banned word or phrase? Replace or cut.
- Passive voice or inanimate actor? Name the person.
- "not X, it's Y" contrast, negative list, or colon reveal? State it directly.
- -ing tail pretending to analyze? Delete or expand with a real consequence.
- Vague attribution or vague declarative ("the implications are significant")? Name the source or the specific thing.
- Three same-length sentences in a row, forced triad, or stacked fragments? Vary.
- Em dash, emoji heading, bold-label bullet, title case? Fix.
- Punchy kicker or recap ending? End on the last concrete point.
- Any sentence that passes to another company unchanged? Cut or specify.
- Any invented fact? Remove it.
- Did the edit strip the writer's voice? Restore it. A light edit or "leave this alone" is a valid output.
