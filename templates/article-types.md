# Article Types

Select the template that matches the content type. If content spans types, pick the primary one.

## Frontmatter (all articles)

Every wiki article starts with YAML frontmatter:

```yaml
---
tldr: "One sentence, under 150 chars. Used for token-efficient index scanning."
---
```

## Counter-Arguments Section (claim-making articles only)

Articles that advocate a framework, strategy, mental model, methodology, or career advice must include a `## Counter-Arguments and Gaps` section (placed above `## Sources`). This section:
- Presents the strongest critique of the main argument
- Notes what data or evidence is missing
- Flags if sources are one-sided

**Skip for:** creative work (poems, dramas), person/entity profiles, pure factual descriptions, transcripts.

---

## Technical / AI

For articles about technology, AI, tools, frameworks, systems.

```
# [Title]

[One-paragraph summary: what this is, why it matters]

## Key Concepts
[Bullet list of core ideas]

## How It Works
[Explanation of mechanisms, processes, architecture]

## Implications
[What this means for the field, for practice, for related topics]

## Sources
- [Source Title](https://original-url) (author/context)

## Related
- [Article Title](relative-path.md) · [[article-name]]
```

**Length target:** 30-80 lines.

---

## Creative Work (Poem, Drama, Story)

For original creative writing or analysis of creative works.

```
# [Title]

[Brief context: who wrote it, when, what prompted it]

## The Work

[The original text, preserved verbatim. Never rewrite creative work.]

## Context
[Historical, biographical, or situational context]

## Themes
[Major themes identified in the work]

## Craft Notes
[Observations on technique, structure, form, language choices]

## Related
[Wiki-links to connected articles]
```

**Length target:** 20-60 lines.
**Rule:** The original work is sacred. Preserve it exactly as written. Analysis goes around it, not through it.

---

## Spiritual / Philosophical

For spiritual teachings, philosophical frameworks, contemplative traditions.

```
# [Title]

[One-paragraph summary of the core teaching or idea]

## Core Teaching
[The central message or principle]

## Interpretation
[Analysis and explanation in accessible language]

## Resonance With Other Traditions
[How this connects to teachings from other traditions or domains]

## Personal Relevance
[Why this matters in practice, how it applies to life or work]

## Sources
[Raw files this draws from]

## Related
[Wiki-links to connected articles]
```

**Length target:** 30-70 lines.

---

## Transcript (Video / Podcast)

For YouTube videos, podcast episodes, interviews, talks.

```
# [Title]

[One-paragraph summary: who spoke, what the main argument was]

## Key Insights
[3-7 bullet points of the most important ideas. Not a full transcript.]

## Speaker Positions
[What each speaker argued or advocated for]

## Takeaways
[Actionable or memorable points]

## Sources
[URL, episode info, raw file reference]

## Related
[Wiki-links to connected articles]
```

**Length target:** 30-60 lines.
**Rule:** Distill, do not transcribe. Capture insights, not every word.

---

## Book / Article Summary

For book highlights, article summaries, written analyses.

```
# [Title]

[One-paragraph summary: the author's central thesis]

## Thesis
[The main argument in 2-3 sentences]

## Key Arguments
[The supporting points, organized logically]

## Notable Quotes
[2-3 direct quotes that carry weight. No more.]

## Assessment
[Strengths, weaknesses, what this contributes to understanding]

## Sources
[Raw file reference, publication info]

## Related
[Wiki-links to connected articles]
```

**Length target:** 30-70 lines.

---

## Person / Entity

For people, organizations, tools, platforms referenced across the wiki.

```
# [Name]

[One-paragraph summary: who/what this is, why it appears in the wiki]

## Role / Relevance
[Why this person or entity matters to the knowledge base]

## Key Ideas
[Their main contributions, positions, or outputs]

## Related
[Wiki-links to connected articles]
```

**Length target:** 20-60 lines.

---

## Concept / Framework

For mental models, frameworks, methodologies, abstract concepts.

```
# [Concept Name]

[One-paragraph definition and significance]

## Definition
[Clear, precise explanation]

## How It Works
[Mechanics, process, application]

## Applications
[Where and how this concept is used]

## Limitations
[Where it breaks down or doesn't apply]

## Sources
[Raw files this draws from]

## Related
[Wiki-links to connected articles]
```

**Length target:** 30-80 lines.

---

## Stub

For topics with too little material for a full article. Stubs get promoted when more material arrives.

```
# [Title]

[2-3 sentence summary of what is known so far]

**Status:** Stub. Needs more material.

## Key Facts
[Bullet list of what is known]

## Related
[Wiki-links to connected articles]
```

**Length target:** 10-15 lines.

---

## Template Selection

Match content type from the raw file's `content_type` frontmatter field:

| content_type | Template |
|-------------|----------|
| web-article, blog-post | Book/Article Summary |
| youtube-transcript, podcast-transcript | Transcript |
| poem, drama, short-story, creative-writing | Creative Work |
| spiritual-text, philosophy | Spiritual/Philosophical |
| paper, technical-doc | Technical/AI or Concept (based on content) |
| tweet-thread, social-post | Book/Article Summary (adapted) |
| book-highlights, book-notes | Book/Article Summary |
| note, idea | Concept/Framework or Stub (based on depth) |

If the content doesn't fit neatly, pick the closest template and adapt. The templates are guides, not straitjackets.

---

## Linking Format (applies to ALL templates)

### Sources Section

Every article must have a `## Sources` section ABOVE `## Related`. Extract ALL URLs from the raw material that fed into this article, including Twitter/X links.

**Non-Twitter sources first:**
```
## Sources
- [Article Title](https://original-url.com)
- [Another Source](https://another-url.com)
```

**Then Twitter/X links below, separated by a blank line:**
```
- [@username on topic](https://twitter.com/username/status/12345)
- [@another_user on related topic](https://twitter.com/another_user/status/67890)
```

Use the line above the Twitter link in the raw file for context on the topic. If no URL exists for a source, use a text description.

### Related Section

Every article ends with `## Related`. Use BOTH Obsidian wiki-links AND clickable markdown links so the wiki works in VS Code/Cursor now and Obsidian later:

```
## Related
- [Article Title](relative-path.md) · [[article-name]]
```

Use relative paths:
- Same domain: `filename.md` (e.g., `hiring-principles.md`)
- Cross-domain: `../otherdomain/filename.md` (e.g., `../thinking/mental-models-for-growth.md`)
