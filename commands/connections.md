# /wiki connections

Deep cross-domain discovery scan. Find non-obvious connections between articles across all wiki domains. This is the highest-priority feature in the wiki system. Hold a high quality bar for every connection you surface.

**Wiki root:** `<WIKI_ROOT>` (see `SKILL.md`)

## Step 1: Load the full inventory

Read `wiki/_index.md` to get the complete list of articles across all domains: ai, productivity, creative, spiritual, research.

If the index has fewer than two domains with articles, stop and tell the user there is not enough content for a cross-domain scan. Suggest they run `/wiki absorb` first if there are unprocessed files in `raw/`.

## Step 2: Load existing connections

Read `wiki/_bridge_log.md`. Parse every entry, including both accepted and rejected connections. Build a set of article-pair keys (sorted alphabetically) so you can skip them later. Never re-suggest a connection that already appears in the bridge log, whether it was accepted or rejected.

## Step 3: Read article content

For each domain that has articles, read each article. For short articles (under 80 lines), read the full text. For longer articles, read the frontmatter and first three paragraphs. You need enough content to identify underlying principles, not just keywords.

## Step 4: Scan domain pairs

Work through every meaningful domain pair in this order:

1. ai + creative
2. ai + spiritual
3. ai + productivity
4. ai + research
5. creative + spiritual
6. creative + productivity
7. creative + research
8. spiritual + productivity
9. spiritual + research
10. productivity + research

For each pair, compare every article in domain A against every article in domain B. For each article pair, ask:

- Do they share an underlying principle? (e.g., "surrender" in Sufi poetry and "letting models explore" in AI training both point to the value of releasing tight control)
- Is there a meaningful contradiction worth examining? (e.g., a productivity framework that demands rigid scheduling vs. a spiritual teaching on stillness and non-striving)
- Does a metaphor from one domain genuinely illuminate the other? The metaphor must do real explanatory work, not just sound clever.
- Does a structural pattern recur across both? (e.g., iterative refinement shows up in drama revision and in model fine-tuning, and the parallel is instructive, not cosmetic)

## Step 5: Apply the quality filter

This step is critical. Most candidate connections are not real connections. Filter ruthlessly.

**Include a connection only if:**
- It reveals a shared deep principle that a reader would not have noticed on their own
- It surfaces a contradiction that, once seen, changes how you think about both articles
- The metaphor carries genuine explanatory weight across domains
- The structural parallel is specific enough to generate new insight

**Reject a connection if:**
- It relies on surface keyword overlap ("both articles mention 'learning'" is not a connection)
- The analogy is forced and would need extensive justification to seem plausible
- It takes three or more logical leaps to connect the two ideas
- It is trivially obvious ("AI research and academic research are both research")
- You would not bring it up in a serious conversation about either topic

When in doubt, leave it out. A scan that returns two high-quality connections is better than one that returns ten mediocre ones.

## Step 6: Score each surviving connection

Assign a confidence level:

- **High:** The connection is specific, surprising, and immediately illuminating once stated. You could write a compelling paragraph about it without straining.
- **Medium:** The connection is real and worth noting, but needs some development to become fully convincing. It points at something true but is not yet crisp.
- **Low:** The connection is plausible and interesting enough to flag, but you are not fully confident it holds up under scrutiny.

## Step 7: Present the report

Show the user a structured report. Do not act on any connections yet.

```
## Cross-Domain Connections Found

Scan date: YYYY-MM-DD
Articles scanned: {count}
Domain pairs examined: {count}
Connections found: {count}

### High Confidence

1. **[[article-a]] (domain) + [[article-b]] (domain)**
   Insight: [the specific non-obvious connection, stated in one to three sentences]
   Recommendation: Create bridge article

### Medium Confidence

1. **[[article-a]] (domain) + [[article-b]] (domain)**
   Insight: [the connection]
   Recommendation: Create bridge article / Add cross-reference only

### Low Confidence

1. **[[article-a]] (domain) + [[article-b]] (domain)**
   Insight: [the connection]
   Recommendation: Flag for future review
```

If the scan found zero connections, say so plainly. Do not manufacture connections to avoid an empty report.

## Step 8: Wait for user approval

Ask the user which connections to act on. Accept responses like:

- "all" or "do all of them"
- "1, 3, 5" (by number)
- "all high" or "high and medium"
- "none" or "skip"

Do not proceed until the user responds.

## Step 9: Create bridge articles for approved connections

For each approved connection:

1. Check if `templates/bridge-article.md` exists. If it does, use it as the template. If it does not, use this structure:

```markdown
---
title: "[Article A title] x [Article B title]"
type: bridge
domains: [domain-a, domain-b]
source_articles:
  - wiki/{domain-a}/{article-a-filename}
  - wiki/{domain-b}/{article-b-filename}
date_created: YYYY-MM-DD
confidence: high/medium/low
tags: [3-5 tags drawn from both source articles]
---

# [Bridge title: a phrase capturing the shared insight]

## The Connection

[Two to four paragraphs explaining the connection. Be specific. Reference concrete details from both source articles. Do not pad with generic observations.]

## From [[article-a]]

[Key passage or idea from article A that anchors this connection]

## From [[article-b]]

[Key passage or idea from article B that anchors this connection]

## What This Opens Up

[One to two paragraphs on questions or directions this connection suggests. What becomes visible once you hold both ideas together?]

## Links

- [[article-a]]
- [[article-b]]
```

2. Generate the bridge filename: `{domain-a}-{domain-b}-{short-descriptor}.md`
   Example: `ai-spiritual-surrender-and-exploration.md`

3. Save to `wiki/bridges/{filename}`.

## Step 10: Update source articles

For each source article involved in an approved bridge:

1. Read the article.
2. If it already has a `## Cross-Domain Links` section, append the new bridge link to it.
3. If it does not, add a `## Cross-Domain Links` section at the end of the article with the bridge link.

Format:
```markdown
## Cross-Domain Links

- [[bridges/{bridge-filename}]] - {one-line description of the connection}
```

## Step 11: Update backlinks

Read `wiki/_backlinks.json`. Add entries for:
- Each bridge article pointing to its two source articles
- Each source article pointing to the bridge article

If `_backlinks.json` does not exist or is empty, create it with the correct structure. The format is:

```json
{
  "wiki/bridges/bridge-filename.md": ["wiki/domain/article-a.md", "wiki/domain/article-b.md"],
  "wiki/domain/article-a.md": ["wiki/bridges/bridge-filename.md"],
  "wiki/domain/article-b.md": ["wiki/bridges/bridge-filename.md"]
}
```

Merge new entries with existing ones. Do not overwrite existing backlinks.

## Step 12: Log all results

Append to `wiki/_bridge_log.md` for every connection in the report, not just approved ones.

Format for each entry:
```markdown
### YYYY-MM-DD

- **[[article-a]] + [[article-b]]** | {confidence} | {accepted/rejected}
  Insight: {the connection}
  Bridge: {bridge filename if created, "none" if rejected}
```

## Step 13: Save the scan report

Save the full report (the output from Step 7, plus a summary of actions taken) to:

`outputs/connections/YYYY-MM-DD-scan.md`

Create the directory if it does not exist. If a file with that name already exists (multiple scans in one day), append `-2`, `-3`, etc.

## Step 14: Update the index

If any bridge articles were created, add them to the `## Bridges` section of `wiki/_index.md`.

## Rules

- Never fabricate connections to fill an empty report. Zero results is a valid outcome.
- Never re-suggest a connection already in the bridge log, accepted or rejected.
- The quality filter in Step 5 is the most important step. Read it twice before scoring.
- Do not write bridge articles in a breathless or grandiose tone. State the connection plainly.
- Keep bridge articles focused. If the connection needs more than four paragraphs to explain, it is probably not a strong connection.
- If the wiki has very few articles (under 5 total), tell the user the scan will be limited and suggest ingesting more material first.
- Always wait for user approval before creating any files.
