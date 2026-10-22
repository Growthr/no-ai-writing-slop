# No AI Writing Slop

A skill for AI writing tools that removes the patterns that make writing read as machine-generated, without changing what the text says or how the writer sounds. It's one SKILL.md file in the open Agent Skills format, so it works in Claude, Codex, Cursor, Gemini CLI, GitHub Copilot and other agents that load skills, and you can paste it into any chatbot that doesn't.

## What it does

The best setup is to install it and [make it automatic](#make-it-automatic), so it runs on everything public-facing you write with AI.

The skill has two modes.

**Edit** is the default. The model makes the smallest edit that removes the tells and returns the revised draft with a short list of what changed. It keeps names, numbers, dates, quotes and code untouched, and it leaves a sentence alone when the writer could defend it. A light edit, or no edit, is a valid result.

**Detect** is for audits. Ask "does this read as AI?" or "audit this for AI patterns" and the model lists each pattern it finds, quotes the line, and suggests a fix in a few words. It doesn't rewrite the text, give a score, or claim a model wrote it.

## What it fixes

- Openers that announce an insight instead of stating it ("Here's what nobody tells you"), and closing lines written to be quoted ("That's the game now.")
- "It's not X. It's Y." contrasts, stacked fragments ("Speed. Iteration. Results.") and colon reveals ("The best part: it learns.")
- 42 words that mark a draft as generated, such as leverage, robust, seamless, delve and ever-evolving, swapped for the plain word
- Inflated claims ("stands as a testament") and sources nobody can check ("experts believe")
- Filler, hedging and "-ing" tails that pretend to analyze ("it's worth noting", "could potentially", "...highlighting its importance")
- Em dashes, emoji, hashtag stacks and bold labels that restate the line
- Chatbot leftovers ("I hope this helps!", "Great question!")
- Rhythm: runs of same-length sentences and paragraphs that all follow one shape

## What it leaves alone

- Names, numbers, dates, URLs, quotes and code
- The writer's own voice. If a phrase is something the writer could defend, it stays
- Qualifiers that carry real scope or risk
- Your style, when you give it a sample. It matches the sample, down to how often you use em dashes

The full rule set is in [SKILL.md](skills/no-ai-writing-slop/SKILL.md).

## Install

### Claude.ai

1. Download [no-ai-writing-slop.zip](https://github.com/Growthr/no-ai-writing-slop/releases/latest/download/no-ai-writing-slop.zip).
2. Turn on code execution in Settings > Capabilities. Skills need it.
3. Go to Customize > Skills, click +, then Create skill > Upload a skill, and choose the zip.

On Team and Enterprise plans, an owner has to allow custom skills first.

### Claude Code

Add this repo as a plugin marketplace, then install the plugin:

```
/plugin marketplace add growthr/no-ai-writing-slop
/plugin install no-ai-writing-slop@growthr
```

To install from your shell instead:

```
claude plugin marketplace add growthr/no-ai-writing-slop
claude plugin install no-ai-writing-slop@growthr
```

To get later versions, run `claude plugin update no-ai-writing-slop@growthr`.

### Codex, Cursor, Gemini CLI and other agents

The [skills CLI](https://skills.sh) installs the skill into whichever agents you have:

```
npx skills add Growthr/no-ai-writing-slop
```

### Any other AI tool

Paste the contents of [SKILL.md](skills/no-ai-writing-slop/SKILL.md) into the tool's custom instructions or project instructions, or into the chat ahead of your draft.

### Manual (Claude Code)

Copy the skill folder into your personal skills directory:

```
git clone https://github.com/Growthr/no-ai-writing-slop.git
cp -r no-ai-writing-slop/skills/no-ai-writing-slop ~/.claude/skills/
```

Claude Code loads it the next time you start a session.

## Make it automatic

Install the skill once and add one line to your global rules. Your AI tool will run it on everything public-facing or written in your voice, without you asking each time:

```
Before showing me any public-facing writing or anything written in my voice (emails, social posts, site copy, docs, proposals), run the no-ai-writing-slop skill on it in edit mode. Run it again after every revision.
```

Where global rules live:

- Claude Code: `~/.claude/CLAUDE.md`
- Codex: `~/.codex/AGENTS.md`
- Gemini CLI: `~/.gemini/GEMINI.md`
- Cursor: Settings > Rules > User Rules
- Claude.ai and ChatGPT: your personal preferences or custom instructions

## Using it

Agents that load skills pick this one up when you ask them to edit or review writing, or when you say "deslop this", "make it sound human" or "remove the AI tells". In Claude Code you can also call it by name: `/no-ai-writing-slop:no-ai-writing-slop` after a plugin install, or `/no-ai-writing-slop` after a manual one. If you add a sample of your own writing, the skill matches it, including how often you use em dashes or words it would otherwise cut.

## Example

A cold email, before:

> Subject: Quick question about your Q4 growth strategy 🚀
>
> Hi Sarah,
>
> I hope this email finds you well! I wanted to reach out because I came across Fernquay's recent Series B announcement — congratulations on this incredible milestone!
>
> In today's ever-evolving fintech landscape, standing out is more crucial than ever. That's where we come in. At Tallowbrook Growth, we leverage cutting-edge AI-powered creative testing to help brands like yours unlock sustainable, scalable growth.
>
> Here's the thing: it's not about spending more. It's about spending smarter.
>
> Last quarter, we helped a fintech client achieve a 3.2x improvement in ROAS while simultaneously cutting CAC by 41%. The results speak for themselves.
>
> Would you be open to a quick 15-minute call next week to explore how we might be able to help Fernquay achieve similar results?
>
> Looking forward to hearing from you!
>
> Best,
> Alex

After:

> Subject: Q4 growth at Fernquay
>
> Hi Sarah,
>
> I saw Fernquay's Series B announcement. Congratulations.
>
> At Tallowbrook Growth, we use AI-powered creative testing to help fintech brands grow. Last quarter, we helped a fintech client improve ROAS 3.2x while cutting CAC by 41%.
>
> Would you be open to a 15-minute call next week to see whether we could do the same for Fernquay?
>
> Best,
> Alex

The Series B, the 3.2x ROAS, the 41% CAC cut and the 15-minute ask stayed. The "hope this finds you well" opener, the "ever-evolving landscape" paragraph, the "not about spending more" line, "The results speak for themselves", the emoji and the em dash went.

## More Growthr skills

The same `growthr` marketplace carries two more free skills: [llms-txt-generator](https://github.com/Growthr/llms-txt-generator) drafts an llms.txt for a website, and [site-scan](https://github.com/Growthr/site-scan) scores a site's SEO and AI readability across 22 checks. Install either with `/plugin install <name>@growthr`, or with `npx skills add Growthr/<name>`.

## Credits

This skill combines ten MIT-licensed skills. Thanks to their authors:

- [stop-slop](https://github.com/hardikpandya/stop-slop) by Hardik Pandya
- [no-ai-slop](https://github.com/petergyang/no-ai-slop) by Peter Yang
- [humanizer](https://github.com/blader/humanizer) by Siqi Chen
- [unslop](https://github.com/cursor/plugins) (pstack) by Lauren Tan
- [slopbeth](https://github.com/ehmo/slopkit) by ehmo
- [humanizer-skill](https://github.com/Aboudjem/humanizer-skill) by Adam Boudjemaa
- [deslop](https://github.com/stephenturner/skills) by Stephen D. Turner
- [anti-slop](https://github.com/elithrar/dotfiles) by Matt Silverlock
- [soundshuman](https://github.com/aashaexo/soundshuman) by aasha
- [anti-ai-slop-writing](https://github.com/jalaalrd/anti-ai-slop-writing) by Jalaaldeen

Their copyright notices are in the skill file itself, in the `metadata.notice` field of SKILL.md's frontmatter. Keep that field if you copy or modify the skill.

## License

MIT. See [LICENSE](LICENSE).

Made by [Growthr](https://growthr.com/tools/no-ai-writing-slop/).
