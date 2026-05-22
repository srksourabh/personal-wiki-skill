# Writing Standards

All wiki articles must follow these standards. No exceptions.

## Anti-Slop Rules

The rules below are self-contained. (If you also have the `linkedin-writer` skill installed at `~/.claude/skills/linkedin-writer/references/style-guide.md`, that file has the canonical version; otherwise this list is sufficient.)

### Banned Punctuation
Em-dashes (--) are banned. Use commas, periods, colons, or parentheses instead. Zero exceptions.

### Banned Phrases

**Throat-clearing openers:**
"Here's the thing:", "The uncomfortable truth is", "It turns out", "I'm going to be honest", "Can we talk about", "Here's what I find interesting"

**Emphasis crutches:**
"Full stop.", "Period.", "Let that sink in.", "This matters because", "Make no mistake", "Read that again."

**Business jargon (use plain language):**
- "Navigate" (challenges) -> "handle", "deal with"
- "Unpack" (analysis) -> "explain", "break down"
- "Lean into" -> "accept", "commit to"
- "Landscape" -> "situation", "field"
- "Game-changer" -> say what specifically changed
- "Deep dive" -> "analysis", "close look"
- "Leverage" (as verb) -> "use"
- "Synergy" -> never
- "Paradigm shift" -> never

**Filler adverbs and empty openers:**
"At its core", "In today's [X]", "In a world where", "It's worth noting", "Interestingly,", "Importantly,", "Crucially,", "At the end of the day", "When it comes to", "The reality is"

**Meta-commentary:**
"Hint:", "Plot twist:", "Spoiler:", "But that's another post", "X is a feature, not a bug"

**AI-overused intensifiers:**
"deeply", "truly", "fundamentally", "inherently", "simply", "literally", "inevitably", "robust", "comprehensive", "cutting-edge", "seamless"

### Banned Structures

**Binary contrast formula:**
- "Not because X. Because Y." -> Just state Y directly.
- "[X] isn't the problem. [Y] is." -> "The problem is Y."
- "The answer isn't X. It's Y." -> State Y.

**Performative simplicity:**
"[Noun]. That's it. That's the [thing]." -> Never.

**False reassurance:**
"And that's okay." / "Not always. Not perfectly." -> Cut these.

**Rhetorical posturing:**
"What if I told you..." -> Make the point directly.
"Think about it." -> Trust the reader.

### Rhythm Anti-Patterns
- Never stack 3+ sentences of identical length in a row.
- Do not end every paragraph with a punchy one-liner. Vary endings.
- Do not always use three-item lists. Two or four items are fine.
- Avoid starting consecutive paragraphs with the same word.

### Authenticity Test
Before finalizing any article, check:
1. Would this sound natural read aloud?
2. Can you identify which sentences sound AI-generated? Rewrite them.
3. Are there phrases a human would never type? Remove them.
4. Does every sentence earn its place? Cut filler.

---

## Wiki-Specific Standards

### Tone
Wikipedia tone. Third-person for factual articles. Creative work analysis and spiritual content can be more personal where appropriate.

Lead with the subject. State facts plainly. Short sentences. Active voice. Plain language.

### Attribution
Attribute claims to sources. "According to [[article-name]]..." rather than stating opinions as facts.

### Quotes
Max 2-3 direct quotes per article. Quotes should carry weight. If a quote doesn't add something the prose can't, cut it.

### Creative Work
Poems, drama excerpts, spiritual texts, and stories are preserved verbatim. The AI adds context, analysis, and craft notes around them. Never rewrite, paraphrase, or "improve" the original work.

### Bridge Articles
The opening paragraph of a bridge article must articulate the specific connection. Not "these two things exist" but "here is the insight at the intersection." If you cannot write that opening paragraph clearly, the connection is not strong enough for a bridge article.

### No Emojis
Wiki articles contain no emojis. Plain text, markdown formatting only.

### Length
Respect the length targets in `templates/article-types.md`. If an article exceeds its target, split it or tighten the prose. Padding is worse than brevity.

### Every Sentence Earns Its Place
If a sentence restates what the previous sentence said, cut it. If a sentence says something obvious, cut it. If a sentence exists only for flow, rewrite the surrounding sentences so the flow works without it.
