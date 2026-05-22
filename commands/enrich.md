# /wiki enrich

Re-process existing wiki articles by fetching their source URLs and rewriting with richer context.

**Wiki root:** `<WIKI_ROOT>` (see `SKILL.md`)

## When to use

Use this command when wiki articles are thin because they were built from bookmark-style raw notes (one-line descriptions + URLs) without the actual source content. This command goes back to the source URLs, fetches the real content, and weaves it into the existing articles.

## Arguments

- `/wiki enrich` -- enrich all wiki articles that have fetchable source URLs.
- `/wiki enrich <domain>` -- enrich only articles in a specific domain (e.g., `thinking`, `startup`).
- `/wiki enrich <article-path>` -- enrich a single article by path (e.g., `wiki/thinking/mental-models-for-growth.md`).

## Enrichment Loop

### Step 1: Build the work list

1. Scan the target articles (all, one domain, or one file depending on arguments).
2. For each article, read the `## Sources` section and extract all URLs.
3. Classify each URL:
   - **Twitter/X** (`twitter.com`, `x.com`) -- fetchable via X API
   - **Web article** (http/https, excluding `notion.so`, `wistia.com`) -- fetchable via WebFetch
   - **YouTube** (`youtube.com`, `youtu.be`) -- fetchable for title/description only
   - **Notion** (`notion.so`) -- skip, not publicly accessible
   - **Video hosting** (`wistia.com`) -- skip
4. Count fetchable URLs per article. Report the work list:

```
## Enrich Plan

Articles to process: {n}
Total URLs to fetch: {n}
  Twitter/X: {n}
  Web articles: {n}
  YouTube: {n}
  Skipped (Notion/other): {n}
```

### Step 2: Process each article

For each article in the work list, run steps a through d in order.

#### a. Fetch source URLs

**Twitter/X URLs:**
1. Extract the tweet ID from the URL (the numeric ID after `/status/`).
2. Fetch via Bash:
   ```bash
   source ~/.follow-builders/.env && curl -s "https://api.x.com/2/tweets/{TWEET_ID}?tweet.fields=author_id,text,created_at,conversation_id&expansions=author_id&user.fields=username,name" -H "Authorization: Bearer $X_BEARER_TOKEN"
   ```
3. Parse the JSON response. Extract: tweet text, author username, creation date.
4. If the tweet is part of a thread (has `conversation_id` different from tweet ID), attempt to fetch the full thread:
   ```bash
   source ~/.follow-builders/.env && curl -s "https://api.x.com/2/tweets/search/recent?query=conversation_id:{CONV_ID}&tweet.fields=author_id,text,created_at&max_results=100" -H "Authorization: Bearer $X_BEARER_TOKEN"
   ```
   Note: Twitter search API only returns recent tweets (~7 days). For older threads, the single tweet fetch is all we get. That is fine.
5. Store the fetched text as enrichment context.

**Web article URLs:**
1. Use WebFetch to retrieve the page.
2. Extract the main article text. Strip navigation, sidebars, ads, footers, cookie banners.
3. Convert to clean text. Preserve headings, lists, key structure.
4. Store as enrichment context.

**YouTube URLs:**
1. Use WebFetch to get the page.
2. Extract: video title, channel name, description text.
3. Store as enrichment context.

**Failure handling:**
- If a URL fetch fails (404, timeout, paywall, rate limit), log the URL and continue.
- Use whatever enrichment context was successfully fetched.
- A partially enriched article is still better than an unenriched one.

#### b. Read the existing article

Read the full wiki article. Parse its structure: frontmatter, sections, content, counter-arguments, sources, related links.

#### c. Rewrite with enrichment

Using BOTH the existing article content AND the fetched enrichment context, rewrite the article:

- **Preserve the existing structure.** Keep the same sections and headings.
- **Weave in new detail.** Add specific arguments, data points, named frameworks, and concrete examples from the fetched sources.
- **Deepen thin sections.** If a section is 2-3 sentences built from a one-line bookmark, expand it with the actual substance from the source.
- **Add attribution.** When adding specific ideas from a fetched source, attribute them (e.g., "Keith Rabois argues that..." or "As Julie Zhuo frames it...").
- **Preserve the TLDR frontmatter.** Update it only if the article's focus has shifted.
- **Preserve the Sources section.** Do not add or remove sources.
- **Preserve the Related section.** Do not change links.
- **Update Counter-Arguments and Gaps.** If fetched content reveals new counter-arguments or fills gaps noted in the existing section, update it.

**Quality rules:**
- Read `templates/writing-standards.md` before rewriting. Follow all anti-slop rules.
- Wikipedia tone. No filler. Every sentence earns its place.
- Max 2-3 direct quotes per article. Paraphrase and synthesize, do not dump fetched text.
- Do not increase article length by more than 2x. If the fetched content is extensive, prioritize the most valuable insights.

#### d. Write the updated article

Write the rewritten article back to the same file path. The article should feel like it was always this detailed, not like new material was bolted on.

### Step 3: Report

After all articles are processed, print a summary:

```
## Enrichment Complete

- Articles enriched: {n}
- URLs fetched successfully: {n}
  - Twitter/X: {n}
  - Web articles: {n}
  - YouTube: {n}
- URLs failed: {n}
- Articles unchanged (no fetchable URLs): {n}

### Failed URLs
- [description](url) -- reason
```

List each enriched article with a one-line note on what changed (e.g., "Added depth to 3 sections from 4 fetched sources").

## Rules

- Always read an article before rewriting it. No blind writes.
- Never remove existing content. Only add depth or correct inaccuracies found in the original sources.
- Never modify the `raw/` folder. This command operates only on `wiki/` articles.
- If `templates/writing-standards.md` does not exist, continue with best judgment.
- Rate limit: pause 1 second between X API calls to avoid hitting rate limits.
- If the X API bearer token is not found in `~/.follow-builders/.env`, warn the user and skip all Twitter/X URLs. Continue with web article URLs.
