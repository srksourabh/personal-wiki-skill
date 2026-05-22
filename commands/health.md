# /wiki health -- On-demand wiki audit

Run a comprehensive quality check across the entire wiki. Produce a structured report with findings and suggested fixes.

## Setup

Wiki root: `<WIKI_ROOT>` (see `SKILL.md`)

Read these files first:
- `wiki/_index.md`
- `wiki/_backlinks.json`
- `wiki/_absorb_log.json`

Then scan every `.md` file inside `wiki/` and its subfolders (skip files prefixed with `_`).

## Checks

Run all seven checks in order.

### 1. Contradictions

Scan articles that reference the same concept or topic. Compare claims, definitions, and descriptions. Flag any pair of articles where statements disagree or conflict. Quote the contradicting lines from each article.

### 2. Orphan articles

Parse `_backlinks.json`. Find every article with zero inbound links. These articles are disconnected from the rest of the wiki and need linking or expanding.

### 3. Unsourced claims

For each wiki article, identify factual assertions, frameworks, or quotes. Check whether supporting material exists somewhere in `raw/`. Flag claims that have no corresponding raw source.

### 4. Bloated articles

Count lines in each wiki article. Compare against these domain targets:

| Domain       | Max lines |
|-------------|-----------|
| Technical   | 80        |
| Creative    | 60        |
| Spiritual   | 70        |
| Bridge      | 50        |
| Person      | 60        |

Flag any article exceeding its domain target. Report current line count vs target.

### 5. Stale content

For each wiki article, check its last-modified date. If the article has not been updated in 30+ days AND newer raw material exists on the same topic, flag it as stale. Report the number of days since the last update.

### 6. Missing articles

Scan all wiki articles for `[[wiki-links]]`. Check whether the linked file actually exists. Flag every broken link with the source article and the missing target.

### 7. Bridge opportunities

List all domain folders under `wiki/`. For each pair of domains, check whether any cross-links exist between them (articles in domain A linking to articles in domain B, or vice versa). Flag domain pairs with zero cross-links. Suggest running `/wiki connections` for those pairs.

## Output

Save the report to `outputs/reports/health-YYYY-MM-DD.md` using today's date.

Use this format:

```markdown
# Wiki Health Check -- YYYY-MM-DD

## Summary
[One paragraph. State the total number of issues found, which checks passed clean, and the most urgent problems.]

## Contradictions
[List each contradiction with the two articles and the conflicting lines. "None found" if clean.]

## Orphan Articles
[List each orphan article path. "None" if clean.]

## Unsourced Claims
[List each unsourced claim with the article and the specific assertion. "None" if clean.]

## Bloated Articles
[List each bloated article with current line count vs target. "None" if clean.]

## Stale Content
[List each stale article with days since last update and the newer raw material that exists. "None" if clean.]

## Missing Articles
[List each broken wiki-link with the source article and missing target. "None" if clean.]

## Bridge Opportunities
[List domain pairs with no cross-links. "None" if all pairs are connected.]

## Suggested Actions
[Prioritized list of fixes. Put the highest-impact, lowest-effort items first. Be specific: name the files to edit and what to do.]
```

## Presentation

After saving the report, present a short summary to the user:
- Total issues found per category
- The top 3 most urgent actions
- The path to the full report file
