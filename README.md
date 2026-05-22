# personal-wiki — a Claude Code skill

A personal knowledge-base wiki for [Claude Code](https://claude.ai/code), inspired by Andrej Karpathy's [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

Drop sources into `raw/`. Claude compiles them into a cross-linked `wiki/` you actually use. Seven `/wiki ...` commands cover the full loop: ingest, organize, query, find connections, audit.

> Built and refined with Claude. Concept credit: Andrej Karpathy.

---

## What it does

| Stage | Commands | What happens |
|---|---|---|
| **Capture** | `/wiki ingest <source>` | Save a URL, file, tweet, YouTube link, or pasted text into `raw/` with auto-generated frontmatter. |
| **Compile** | `/wiki absorb` · `/wiki enrich` | Turn un-absorbed `raw/` entries into Wikipedia-style `wiki/` articles. Cross-link them. Fetch source URLs for depth. |
| **Recall** | `/wiki query <question>` | Answer questions grounded only in your wiki articles, with citations. Optionally promote answers back into the wiki. |
| **Discover** | `/wiki connections` | Find non-obvious cross-domain bridges (e.g. an AI paper that connects to a Sufi poem). Quality-filtered, not noisy. |
| **Audit** | `/wiki health` · `/wiki status` | Find orphans, contradictions, broken links, bloated articles. Print a dashboard of vault stats. |

## Why this exists

Most note systems are write-only. The LLM Wiki pattern flips that: notes are **compiled** into a structured artifact the model maintains alongside you. Same way you compile source code once instead of re-deriving it on every keystroke, the wiki compiles context once instead of re-deriving it on every prompt.

For ~hundreds of pages of personal knowledge, this beats RAG: cheaper tokens, better synthesis, fully editable in plain markdown, and you can actually *read* what the AI built.

---

## Install

This is a Claude Code skill. Three options:

### Option A — Drop into your skills folder

```bash
git clone https://github.com/kvsdileep/personal-wiki-skill ~/.claude/skills/personal-wiki
```

### Option B — Install as a plugin

```bash
# In Claude Code:
/plugin marketplace add kvsdileep/personal-wiki-skill
/plugin install personal-wiki
```

### Option C — Symlink (if you want to keep editing)

```bash
git clone https://github.com/kvsdileep/personal-wiki-skill ~/code/personal-wiki-skill
ln -s ~/code/personal-wiki-skill ~/.claude/skills/personal-wiki
```

---

## Configure

After installing, **set your wiki root path** by editing the placeholder in these files:

```bash
cd ~/.claude/skills/personal-wiki
grep -rl "<WIKI_ROOT>" . | xargs sed -i '' "s|<WIKI_ROOT>|$HOME/Documents/personal_wiki|g"
```

Replace `$HOME/Documents/personal_wiki` with wherever you want your vault to live. The skill will create `raw/` and `wiki/` subfolders inside it on first use.

> **Tip:** Point this at an Obsidian vault and you get a polished UI for free — the graph view, backlinks panel, and search all work out of the box.

---

## First run

```
You: /wiki ingest https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
Claude: Ingested: LLM Wiki — Karpathy
        Type: article / web-article
        Tags: llm-wiki, knowledge-management, karpathy
        Saved: raw/articles/llm-wiki-karpathy.md

You: /wiki absorb
Claude: Found 1 un-absorbed entry. Processing...
        Created: wiki/ai/llm-wiki-pattern.md
        Updated wiki/_index.md
        Absorption complete.

You: /wiki query What's the core insight of the LLM Wiki?
Claude: According to [[llm-wiki-pattern]], context should be compiled once and
        maintained, not re-derived on every prompt...
```

---

## Optional dependencies

- **X/Twitter ingestion** — `/wiki ingest <tweet-url>` and the URL-enrichment step in `/wiki absorb` use the X API. They expect a bearer token at `~/.follow-builders/.env` as `X_BEARER_TOKEN`. If you don't have it, those URLs are skipped gracefully — everything else still works.
- **Writing standards** — `templates/writing-standards.md` is self-contained. If you also have the [`linkedin-writer`](https://github.com/) skill installed, it'll prefer that file as the canonical source; otherwise the template's own rules are applied.

---

## Folder layout

```
personal-wiki/
├── SKILL.md                       ← entry point Claude reads
├── commands/
│   ├── ingest.md                  ← /wiki ingest
│   ├── absorb.md                  ← /wiki absorb
│   ├── enrich.md                  ← /wiki enrich
│   ├── query.md                   ← /wiki query
│   ├── connections.md             ← /wiki connections
│   ├── health.md                  ← /wiki health
│   └── status.md                  ← /wiki status
├── reference/
│   ├── domains.md                 ← AI / productivity / creative / spiritual / research
│   └── ingest-formats.md          ← per-format ingestion rules
└── templates/
    ├── article-types.md           ← concept / person / tool / etc.
    ├── bridge-article.md          ← cross-domain bridges
    └── writing-standards.md       ← anti-slop rules
```

Each command file is read on demand. The skill doesn't load everything at once — it loads what it needs when it needs it.

---

## What it won't do

- Scale past ~hundreds of articles without performance degrading
- Stay perfect without occasional `/wiki health` runs
- Work if you feed it junk sources (garbage in, organized garbage out)
- Stay private once you query — the relevant pages are sent to Claude

---

## Credits

- **Concept:** [Andrej Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) is the seed idea.
- **Implementation:** Built and iterated by [K V S Dileep](https://github.com/kvsdileep) with Claude.
- **Pattern context:** Builds on Tiago Forte's [Building a Second Brain](https://fortelabs.co/blog/para/) (CODE + PARA).

---

## License

MIT — see [LICENSE](./LICENSE).

Use it, fork it, sell it, rewrite it. Attribution appreciated, not required.
