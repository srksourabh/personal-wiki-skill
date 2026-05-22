# /wiki absorb

Compile un-absorbed raw entries into organized wiki articles with cross-domain connection discovery.

**Wiki root:** `<WIKI_ROOT>` (see `SKILL.md`)

## Absorption Loop

Process entries sequentially. Do not parallelize. Each entry must finish before starting the next.

### Step 1: Scan for un-absorbed entries

1. Read `wiki/_absorb_log.json`.
2. Scan all files in `raw/` and its subfolders (`raw/articles/`, `raw/books/`, `raw/creative/`, `raw/notes/`, `raw/research/`, `raw/social/`, `raw/transcripts/`).
3. An entry needs absorption if:
   - Its YAML frontmatter has `absorbed: false`, OR
   - It does not appear in `_absorb_log.json` at all.
4. Collect the list. Report how many entries need absorption before starting.

### Step 2: Process each un-absorbed entry

For each entry, run steps a through f in order.

#### a. Read

Read the raw file completely. Parse its frontmatter and body content.

#### a2. Enrich -- Fetch source URLs

Scan the raw file body for URLs (http/https links). For each URL, classify and fetch:

**Twitter/X URLs** (`twitter.com` or `x.com`):
1. Extract the tweet ID from the URL.
2. Source the X API bearer token: `source ~/.follow-builders/.env`
3. Fetch via Bash:
   ```bash
   source ~/.follow-builders/.env && curl -s "https://api.x.com/2/tweets/{TWEET_ID}?tweet.fields=author_id,text,created_at,conversation_id&expansions=author_id" -H "Authorization: Bearer $X_BEARER_TOKEN"
   ```
4. Parse the JSON response. Extract: tweet text, author username, creation date.
5. If the tweet is part of a thread (has `conversation_id`), fetch the full thread:
   ```bash
   source ~/.follow-builders/.env && curl -s "https://api.x.com/2/tweets/search/recent?query=conversation_id:{CONV_ID}&tweet.fields=author_id,text,created_at&max_results=100" -H "Authorization: Bearer $X_BEARER_TOKEN"
   ```
6. Store the fetched text as enrichment context for this entry.

**Web article URLs** (any http/https, excluding `notion.so`, `wistia.com`):
1. Use WebFetch to retrieve the page.
2. Extract the main article text. Strip navigation, sidebars, ads, footers.
3. Store the extracted text as enrichment context.

**YouTube URLs** (`youtube.com`, `youtu.be`):
1. Use WebFetch to get the page.
2. Extract: video title, channel name, description.
3. Store as enrichment context. (Full transcripts require the user to paste them.)

**Skip these URL types:**
- `notion.so` links (private, not publicly accessible)
- `wistia.com` links (video hosting, cannot extract text)
- Any URL that returns an error or is behind a paywall -- log it and move on.

**Failure handling:**
- If any URL fetch fails, log the URL and continue. Do not block absorption.
- Use whatever enrichment context was successfully fetched.
- In the final report (Step 4), list any URLs that could not be fetched.

**Using enrichment context:**
- Pass all fetched content to step d (Create or update the article) as additional source material.
- The article should be written using BOTH the raw file's notes AND the fetched content.
- Fetched content provides depth: specific arguments, data points, quotes, frameworks.
- The one-line description in the raw file provides the topic framing.
- Do not dump fetched content verbatim. Synthesize it into the article using Wikipedia-style prose.

#### b. Classify

Identify which domain(s) the content belongs to: `ai`, `productivity`, `creative`, `spiritual`, `research`.

Read `reference/domains.md` for the taxonomy and classification guidance. Use it to resolve ambiguous cases. Content can belong to multiple domains. Pick a primary domain for file placement.

#### c. Match against existing wiki

Read `wiki/_index.md` to find articles this content relates to.

Decide: does this content update an existing article, or does it warrant a new one?

- If the content adds depth, corrects, or extends an existing article: update that article.
- If the content covers a distinct topic not yet in the wiki: create a new article.
- If the content touches multiple existing articles: update each one, but put the bulk of new material where it fits best.

#### d. Create or update the article

Read `templates/article-types.md` to pick the right template based on content type (concept, technique, person, tool, etc.).

Read `templates/writing-standards.md` for quality rules. Follow them strictly. No AI slop. Wikipedia-style tone. No filler.

**If creating a new article:**
- Place it in the appropriate domain folder: `wiki/{domain}/{article-name}.md`
- Filename: lowercase, hyphenated. Example: `transformer-architecture.md`
- Apply the correct template structure from `templates/article-types.md`.
- Add a `tldr:` field to the YAML frontmatter (one sentence, under 150 chars). This lets the index scan skip full reads.

**If updating an existing article:**
- Read the existing article first. Always.
- Weave new information into the existing structure. Do not append a new section at the bottom. Integrate facts, nuance, and sources into the sections where they belong.
- Preserve existing `[[wiki-links]]` and the `## Related` section.
- Update the `tldr:` frontmatter if the article's focus has shifted.

**Bias check (for articles making claims or advocating frameworks):**
After writing or updating a concept, framework, or strategy article, add a `## Counter-Arguments and Gaps` section. This section should:
- Present the strongest critique of the main argument
- Note what data or evidence is missing
- Flag if the sources are one-sided (e.g., 5 articles all praising the same approach)

Skip this section for creative work (poems, dramas), person/entity profiles, and pure factual descriptions. Apply it to: frameworks, strategies, mental models, methodologies, product approaches, career advice.

#### e. Reweave pass (cross-domain connection discovery)

This is the most important step in the entire process.

After creating or updating the article:

1. Identify the domains this article does NOT belong to.
2. Scan `wiki/_index.md` for 3-5 articles in those other domains that might connect to this one.
3. Read each candidate article.
4. For each genuine connection found:
   - Add a `## Cross-Domain Links` section to both articles. Each entry: `[[wiki-link]]` followed by a one-sentence explanation of the connection.
   - If the connection is substantial, create a bridge article in `wiki/bridges/` using `templates/bridge-article.md`.
   - Log the connection in `wiki/_bridge_log.md` with: date, source article, target article, and the insight.

**What counts as a real connection:**
- A shared underlying principle operating across domains
- A meaningful contradiction worth examining
- A metaphor from one domain that genuinely illuminates another
- A pattern recurring across unrelated domains

**What does NOT count. Reject these:**
- Surface-level keyword overlap ("both mention learning")
- Forced analogies that require squinting
- Connections requiring three leaps of logic to justify
- Generic observations ("both involve iteration")

When in doubt, skip the connection. A sparse bridge log with real insights beats a bloated one with noise.

#### f. Update tracking

Do all of the following after each entry:

1. **Update `wiki/_index.md`:** Add or update the entry for the article. Include the path and a one-line description.
2. **Update `wiki/_backlinks.json`:** Add all new `[[wiki-links]]` created or modified during this entry's processing.
3. **Update `wiki/_absorb_log.json`:** Add an entry recording the raw file path, target article(s), timestamp, and action taken (created/updated).
4. **Mark the raw file:** Set `absorbed: true` in the raw file's YAML frontmatter. Do not modify anything else in the raw file.

### Step 3: Checkpoint

Every 10 entries, rebuild `wiki/_index.md` from scratch by scanning all files in `wiki/ai/`, `wiki/productivity/`, `wiki/creative/`, `wiki/spiritual/`, `wiki/research/`, and `wiki/bridges/`. This prevents the index from drifting out of sync with actual wiki contents.

### Step 4: Report

After all entries are processed, print a summary:

```
## Absorption Complete

- Entries absorbed: {n}
- Articles created: {n}
- Articles updated: {n}
- Bridge articles created: {n}
- Cross-domain connections found: {n}
```

List each new bridge article with its one-line insight.

## Rules

- Filename format: lowercase, hyphenated. Example: `transformer-architecture.md`
- Always read an article before editing it. No blind writes.
- Never modify files in `raw/` except to set `absorbed: true` in frontmatter.
- If `reference/domains.md`, `templates/article-types.md`, `templates/writing-standards.md`, or `templates/bridge-article.md` does not exist yet, warn the user and continue with best judgment. Do not halt the process.

## Linking format

**Sources section:** Every article must have a `## Sources` section (above `## Related`) with original URLs from the raw material. Extract ALL URLs from the raw files that fed into the article, including Twitter/X links. Format:

For articles and blog posts:
```
- [Article Title](https://url)
```

For Twitter/X links:
```
- [@username on topic](https://twitter.com/username/status/12345)
```

Use the line above the Twitter link in the raw file for context on the topic. If the raw file has `[https://twitter.com/...](https://twitter.com/...)` with a description on the preceding line, use that description. Group non-Twitter sources first, then Twitter sources below with a blank line separator.

**Related section:** Every article ends with `## Related`. Use BOTH Obsidian wiki-links and clickable markdown links:
```
- [Article Title](relative-path.md) · [[article-name]]
```
Use relative paths: same domain = `filename.md`, cross-domain = `../otherdomain/filename.md`.
