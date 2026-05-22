# /wiki query <question>

Answer a user's question using only the wiki's content. Ground every claim in specific articles.

**Wiki data:** `<WIKI_ROOT>` (see `SKILL.md`)

## Process

### 1. Load the wiki inventory

Read `wiki/_index.md` to get the full list of articles grouped by domain.

Read `wiki/_backlinks.json` to identify which topics have the most inbound links. Articles with many backlinks are authoritative on their topic -- prefer them.

### 2. Select relevant articles

Pick 3-8 articles that are most likely to answer the question. Prioritize:

- **Direct topic matches** -- article title or domain matches the question's subject
- **High-backlink articles** -- heavily referenced articles carry more weight
- **Bridge articles** (`wiki/bridges/`) -- use these when the question spans multiple domains (e.g., "How does meditation relate to creativity?")

If the question touches multiple domains, always check `wiki/bridges/` for existing cross-domain articles.

### 3. Read and follow links

Read each selected article in full.

If an article references other articles via `[[wiki-links]]` that seem relevant to the question, follow those links. Go up to 2-3 levels deep, but stop when you have enough context. Do not read the entire wiki.

### 4. Synthesize the answer

Write a clear, grounded answer:

- **Cite every claim.** Use the format: "According to [[article-name]], ..." or parenthetical references like "([[article-name]])".
- **Handle disagreements.** If two articles present conflicting views, state both: "[[article-a]] argues X, while [[article-b]] suggests Y."
- **Admit gaps.** If the wiki does not contain enough information to fully answer the question, say so explicitly. Never fabricate information that is not in the wiki.
- **No AI slop.** No em-dashes. No filler phrases. No "it's worth noting" or "interestingly." Write plainly.
- If `templates/writing-standards.md` exists in the wiki data directory, apply those standards to the answer.

### 5. Save the output

Create the output file at:

```
outputs/queries/YYYY-MM-DD-[slug].md
```

Where `[slug]` is a short, lowercase, hyphenated version of the question (e.g., "how-transformers-work").

Use this format:

```markdown
# Query: [the original question]

**Date:** YYYY-MM-DD
**Articles consulted:** [[article-1]], [[article-2]], ...

## Answer

[the synthesized answer with inline citations]
```

### 6. Offer to promote to wiki

After presenting the answer, ask: "This answer could become a wiki article. Want me to add it to the wiki?"

If the user says yes:
1. Pick the right domain and filename
2. Restructure the answer into a proper wiki article using `templates/article-types.md`
3. Add `tldr:` frontmatter, `## Sources`, and `## Related` sections
4. If the article makes claims or advocates a position, add `## Counter-Arguments and Gaps`
5. Update `wiki/_index.md` and `wiki/_backlinks.json`
6. Report where the new article was created

This closes the loop: questions become knowledge that improves future answers.

### 7. Present the result

Show the answer directly to the user. At the end, mention the saved output file path and the option to promote it to a wiki article.

## Rules

- **Only use wiki/ articles.** Never pull information from `raw/` files. Raw material is unprocessed and may be incomplete or unverified.
- **Check bridges.** If the question spans domains, look in `wiki/bridges/` for cross-domain articles before answering.
- **Handle an empty wiki.** If `wiki/_index.md` is empty or lists very few articles, tell the user the wiki is too thin to answer the question. Suggest running `/wiki ingest` and `/wiki absorb` first.
- **Keep answers concise but thorough.** Answer the question fully without padding. Prefer short paragraphs over long walls of text.
- **Create the output directory if needed.** If `outputs/queries/` does not exist, create it before saving.
