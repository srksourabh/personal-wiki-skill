# Ingest Format Handling

Reference for the `/wiki ingest` command. Covers how to handle each input type.

## Input Detection

Determine input type in this order:
1. Starts with `http://` or `https://` -> URL
2. Path exists on filesystem and is a directory -> Folder
3. Path exists on filesystem and is a file -> File
4. Everything else -> Pasted text

## URL Handling

### Web Articles
Use WebFetch to get the page content. Extract the article body:
- Strip navigation, sidebar, footer, ads, cookie banners
- Preserve headings, paragraphs, lists, code blocks, images
- Extract the page title from `<title>` or `<h1>`
- Convert to clean markdown

If WebFetch fails (paywall, JavaScript-heavy, timeout), report the error and suggest the user copy-paste the text manually.

### YouTube URLs
Extract the video ID from the URL. Save:
- Title (from page metadata)
- Channel name
- URL
- Description if available

Note: Transcript extraction requires the user to paste the transcript separately or use a transcript tool. Save whatever is available. Mark as `content_type: youtube-transcript` if transcript is included, `content_type: youtube-reference` if only metadata.

### Tweet / Thread URLs
Use WebFetch. Extract:
- Tweet text (full content)
- Author handle and name
- Date
- For threads: all tweets in order, separated by `---`
- Quote tweets: include the quoted content

### Other URLs
Treat as a web article. Fetch, extract text, save as markdown.

## File Handling

### .md files
Read as-is. Check if YAML frontmatter already exists. If it does, merge with the standard frontmatter (keep existing fields, add missing ones). If not, add frontmatter.

### .txt files
Read content. Wrap in markdown. Add frontmatter. Auto-detect content type from the text (poem, note, article, etc.).

### .pdf files
Use the Read tool with PDF support. Convert extracted text to markdown, preserving structure (headings, lists, tables where possible). For large PDFs, read page by page and note the page range in frontmatter.

### .html files
Strip HTML tags. Extract text content. Preserve structure (headings from h1-h6, lists from ul/ol, paragraphs). Convert to markdown. Add frontmatter.

### .json files
If it's structured data: summarize the structure and key data points in markdown. Include a sample of the data.
If it's an export from another app (e.g., bookmarks, notes export): parse and extract the content.

### .csv / .tsv files
Read the file. Summarize columns and row count. Convert key data to a markdown table (first 10-20 rows if large). Note the full size.

### Images (.png, .jpg, .gif, .webp)
Use the Read tool to view the image. Write a description of the image content in markdown. Reference the original image path. Set `content_type: image`.

## Pasted Text

When the user provides text directly (not a URL or file path):
1. Auto-detect content type from the text itself:
   - Has verse structure, line breaks, stanzas -> poem
   - Has dialogue markers, stage directions -> drama
   - Has citations, methodology -> research
   - Has code blocks, technical terms -> technical article
   - Short, informal -> note
   - Default -> article
2. Generate a title from the first line or key phrase
3. Add frontmatter with `source: "pasted"`

## Frontmatter Generation

Every ingested file gets this YAML frontmatter:

```yaml
---
title: [extracted from content or generated]
date_ingested: YYYY-MM-DD
source: [URL, file path, or "pasted"]
source_type: [article|note|transcript|research|social|creative|book]
content_type: [specific type, see list below]
tags: [3-5 auto-generated tags based on content]
absorbed: false
---
```

### content_type values
- web-article, blog-post
- youtube-transcript, youtube-reference, podcast-transcript
- poem, drama, short-story, creative-writing, spiritual-text
- paper, technical-doc
- tweet-thread, social-post
- book-highlights, book-notes
- note, idea, image

### source_type routing

| source_type | Save to |
|------------|---------|
| article | raw/articles/ |
| note | raw/notes/ |
| transcript | raw/transcripts/ |
| research | raw/research/ |
| social | raw/social/ |
| creative | raw/creative/ |
| book | raw/books/ |

## File Naming

- Lowercase, hyphenated, derived from title
- Example: "Transformer Architecture Explained" -> `transformer-architecture-explained.md`
- If file exists, append a number: `transformer-architecture-explained-2.md`
- Max 60 characters for the filename (before .md)

## Bulk Mode

When given a folder path:
1. List all files in the folder (non-recursive by default, recursive if user specifies)
2. Skip files already in `wiki/_absorb_log.json`
3. Process each file using the rules above
4. Report: total files found, files processed, files skipped, any errors
