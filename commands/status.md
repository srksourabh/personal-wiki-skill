# /wiki status -- Quick wiki dashboard

Display a compact overview of the wiki's current state. No files saved. Just print the dashboard directly to the user.

## Setup

Wiki root: `<WIKI_ROOT>` (see `SKILL.md`)

## Data to collect

Gather all of the following before producing output.

### 1. Article count by domain

Count `.md` files in each subfolder of `wiki/`. Exclude any file prefixed with `_` (like `_index.md`, `_backlinks.json`). Group counts by subfolder name.

### 2. Bridge count

Count `.md` files in `wiki/bridges/`. These are cross-domain connection articles.

### 3. Un-absorbed entries

Check files in `raw/` and its subfolders. Cross-reference against `wiki/_absorb_log.json`. Count entries where `absorbed` is `false` or where the raw file does not appear in the log at all. Also count total raw source files.

### 4. Recent activity

Read `wiki/_index.md`. Find the "Recently Added" section or equivalent. Count articles added or updated in the last 7 days. Separate article count from bridge count.

### 5. Most-connected articles

Read `wiki/_backlinks.json`. Sort articles by number of inbound links, descending. Take the top 5. Format as `[[article-name]] (N links)`.

### 6. Orphan count

From `_backlinks.json`, count articles with zero inbound links.

### 7. Total raw sources

Count all files across all subfolders of `raw/`.

## Output format

Print exactly this structure, filling in real numbers:

```
Wiki Status (YYYY-MM-DD)
----------------------------------------
Articles:  N total (Domain1: n, Domain2: n, ...)
Bridges:   N cross-domain connections
Raw:       N un-absorbed entries pending (M total sources)
Recent:    +N articles this week, +N bridges
Top nodes: [[name]] (N links), [[name]] (N links), ...
Orphans:   N
```

Use today's date. Use a line of dashes as the separator.

If any data source is missing (e.g., `_backlinks.json` does not exist yet), note it inline rather than failing. For example: `Top nodes: (no backlinks data yet)`.

Do not save this output to a file. Print it directly.
