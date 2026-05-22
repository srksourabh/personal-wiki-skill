# Bridge Article Template

Use this template when creating cross-domain connection articles in `wiki/bridges/`.

## When to create a bridge article

Only when a connection meets the quality bar:
- Shared underlying principle across domains
- Meaningful contradiction worth examining
- A metaphor from one domain that genuinely illuminates another
- A structural pattern recurring in unrelated domains

Do NOT create bridge articles for keyword overlap, forced analogies, or connections requiring multiple logical leaps.

## Template

```markdown
# [Connection Title]: [Domain A Concept] and [Domain B Concept]

[Opening paragraph. This is the most important part. Articulate the
specific insight that emerges at the intersection. Not "these two
things exist" but "here is what you learn by seeing them together."
If you cannot write this paragraph clearly, the connection is not
strong enough for a bridge article.]

## From [Domain A]
[What Domain A says about this, with [[wiki-links]] to source articles.
Summarize the relevant ideas. Do not rehash the entire source article.]

## From [Domain B]
[What Domain B says about this, with [[wiki-links]] to source articles.
Same principle: relevant ideas only.]

## The Insight
[The specific, non-obvious observation. What emerges from seeing these
together that neither domain reveals alone. This section should make
the reader think "I never would have connected those."]

## Open Questions
[1-3 questions this connection raises. These become seeds for future
exploration and potential new bridge articles.]

## Related
[[source-article-a]]
[[source-article-b]]
[Any other connected wiki articles]
```

## Rules

- Length target: 20-50 lines
- The opening paragraph determines whether the article should exist. Write it first. If it reads forced, do not create the article.
- Both "From" sections must link back to existing wiki articles
- "The Insight" section is where the value lives. Be specific. Avoid vague claims about "parallels."
- "Open Questions" seeds future connections. Frame them as genuine questions, not rhetorical ones.
- Follow all rules in `templates/writing-standards.md`

## Naming

Filename should capture the connection: `iterative-spiral-drama-ai.md`, `surrender-alignment.md`, `stillness-and-productivity.md`

Save to: `wiki/bridges/[filename].md`

## After creating a bridge article

1. Add `## Cross-Domain Links` section to both source articles with a `[[link]]` to this bridge article and a one-sentence explanation
2. Update `wiki/_backlinks.json`
3. Log in `wiki/_bridge_log.md`:
   ```
   ## YYYY-MM-DD
   - **[[source-a]]** <-> **[[source-b]]** -> Created [[bridge-name]]
     Insight: [one sentence]
   ```
4. Update `wiki/_index.md` under the Bridges section
