---
name: personal-wiki
description: Build and maintain a personal knowledge base wiki. Use this skill whenever the user mentions wiki, knowledge base, ingest, absorb, personal wiki, or asks to add content to their wiki. Also use when the user asks questions like "what does my wiki say about X", wants to find connections between topics, run a wiki health check, or check wiki status. Trigger on any /wiki command. This skill manages ingesting raw content (URLs, files, text), compiling it into organized wiki articles, querying across the knowledge base, discovering cross-domain connections, and running quality audits.
---

# Personal Wiki

Multi-domain personal knowledge base with cross-domain connection discovery.

**Wiki data:** `<WIKI_ROOT>` — set this to the absolute path of your wiki folder (e.g. `~/Documents/personal_wiki/`). The skill will create `raw/` and `wiki/` subfolders inside it.

## Commands

| Command | Read first | Purpose |
|---------|-----------|---------|
| `/wiki ingest [source]` | `commands/ingest.md` | Ingest URLs, files, or text into raw/ |
| `/wiki absorb` | `commands/absorb.md` | Compile raw/ entries into wiki articles |
| `/wiki enrich` | `commands/enrich.md` | Fetch source URLs and deepen existing articles |
| `/wiki query <question>` | `commands/query.md` | Ask questions against the wiki |
| `/wiki connections` | `commands/connections.md` | Discover cross-domain bridges |
| `/wiki health` | `commands/health.md` | Audit wiki quality |
| `/wiki status` | `commands/status.md` | Show dashboard stats |

Read the relevant command file before executing any command. Command files reference templates and reference docs as needed.
