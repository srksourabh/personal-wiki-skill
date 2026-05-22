# /wiki ingest

Ingest raw material into the personal wiki. Accept any input format, convert to clean markdown with frontmatter, and save to the correct `raw/` subfolder.

**Wiki root:** `<WIKI_ROOT>` (see `SKILL.md`)

## Step 1: Detect input type

Look at what the user provided and classify it:

- **Web URL** (starts with http/https, not YouTube or Twitter/X) -- treat as web article
- **YouTube URL** (youtube.com or youtu.be) -- treat as YouTube content
- **Tweet/X URL** (twitter.com or x.com) -- treat as social/tweet thread
- **File path** (ends in .md, .txt, .pdf, .html, .json, .csv, or image extension) -- treat as local file
- **Folder path** (a directory) -- treat as bulk ingest
- **Pasted text** (none of the above) -- treat as raw text

If the user did not provide any input, ask them what they want to ingest.

## Step 2: Fetch or read the content

### Web article URL

1. Use the WebFetch tool to retrieve the page.
2. Extract the main article text. Strip navigation, sidebars, footers, ads, cookie banners, and other non-content elements.
3. Convert the extracted text to clean markdown. Preserve headings, lists, code blocks, bold/italic, and links.
4. Extract the article title from the `<title>` tag or first `<h1>`.
5. Extract the publication date if visible.
6. If WebFetch fails, tell the user the fetch failed and suggest they copy-paste the article text directly.

### YouTube URL

1. Extract the video ID from the URL.
2. Use WebFetch to get the page title.
3. Save the URL, video title, and channel name.
4. If the user also provides a transcript (pasted text), include it in the body.
5. If no transcript is provided, note that in the file body and suggest the user paste the transcript later.

### Tweet/X URL

1. Use WebFetch to retrieve the tweet page.
2. Extract: author handle, display name, tweet text, date, and any media descriptions.
3. If it is a thread, preserve all tweets in order. Number each tweet in the thread.
4. Convert to clean markdown.

### File path

1. Use the Read tool to read the file.
2. For `.md` and `.txt` files: use content as-is.
3. For `.pdf` files: use Read with page ranges if large. Extract all text.
4. For `.html` files: strip tags, extract text content, convert to markdown.
5. For `.json` and `.csv` files: convert to a readable markdown table or structured format.
6. For images: save the file reference and describe what you can see in the image.
7. Extract or generate a title from the content.

### Folder path (bulk mode)

1. Use Bash to list all files in the folder (non-recursive by default, recursive if user asks).
2. Read `wiki/_absorb_log.json` if it exists. Skip any files already listed there.
3. Process each file individually using the rules above.
4. Report progress as you go: "Ingesting file 3 of 12..."

### Pasted text

1. Auto-detect what the content is:
   - Has verse structure, line breaks, stanzas --> creative (poem/lyrics)
   - Has dialogue format, character names --> creative (drama/screenplay)
   - Has citations, abstract, methodology --> research (paper)
   - Has bullet points, personal reflections --> note
   - Has code blocks, technical terms --> article (technical)
   - Has timestamps, speaker labels --> transcript
   - Default: note
2. Generate a title from the first line or a summary of the content.

## Step 3: Build the frontmatter

Every ingested file gets YAML frontmatter at the top. Determine these fields:

```yaml
---
title: [extracted or generated title -- concise, descriptive]
date_ingested: [today's date in YYYY-MM-DD format]
source: [the URL, file path, or "pasted"]
source_type: [one of: article, note, transcript, research, social, creative, book]
content_type: [specific subtype, see list below]
tags: [3-5 auto-generated tags based on content]
absorbed: false
---
```

### source_type and content_type mapping

| source_type | content_type options |
|-------------|---------------------|
| article | web-article, blog-post, newsletter, technical-article, opinion-piece |
| note | personal-note, meeting-notes, book-highlights, idea, reflection |
| transcript | youtube-transcript, podcast-transcript, lecture-transcript, interview |
| research | paper, whitepaper, report, case-study, survey |
| social | tweet-thread, tweet, social-post |
| creative | poem, short-story, essay, drama, lyrics, spiritual-text |
| book | book-highlights, book-summary, book-chapter |

Pick the most specific content_type that fits.

### Tags

Generate 3 to 5 tags. Use lowercase, hyphenated terms. Draw tags from the actual content, not generic terms. Good: `transformer-architecture`, `attention-mechanism`, `nlp`. Bad: `interesting`, `technology`, `article`.

## Step 4: Generate the filename

1. Derive the filename from the title.
2. Lowercase everything.
3. Replace spaces and special characters with hyphens.
4. Remove articles (a, an, the) from the start.
5. Trim to 60 characters max.
6. Add `.md` extension.
7. Example: "The Transformer Architecture Explained" --> `transformer-architecture-explained.md`

Check if the file already exists in the target subfolder. If it does, append `-2` (or `-3`, etc.) before the extension.

## Step 5: Save the file

Route to the correct subfolder based on source_type:

| source_type | Save to |
|-------------|---------|
| article | `raw/articles/` |
| note | `raw/notes/` |
| transcript | `raw/transcripts/` |
| research | `raw/research/` |
| social | `raw/social/` |
| creative | `raw/creative/` |
| book | `raw/books/` |

Write the file using the Write tool. The full path is: `<WIKI_ROOT>/raw/{subfolder}/{filename}`

The file content is: frontmatter block + blank line + the cleaned markdown content.

## Step 6: Report the result

After ingesting, print a summary:

```
Ingested: {title}
  Type: {source_type} / {content_type}
  Tags: {tag1}, {tag2}, {tag3}
  Saved: raw/{subfolder}/{filename}
```

For bulk ingests, print individual results, then a final summary:

```
Bulk ingest complete:
  Files processed: {count}
  Skipped (already ingested): {count}
  Failed: {count}

  Breakdown:
    articles: {count}
    notes: {count}
    ...
```

## Rules

- Never overwrite existing files in `raw/` unless the user explicitly says "re-ingest".
- Always generate frontmatter. No exceptions.
- If content is ambiguous, default to `note` for source_type.
- If a URL fetch fails, do not silently skip it. Report the error and suggest the user paste the content manually.
- For bulk ingests, if one file fails, continue with the rest. Report failures at the end.
- See `reference/ingest-formats.md` for detailed format-specific handling when it exists.
- Keep the converted markdown clean. No HTML artifacts, no escaped characters that break readability.
