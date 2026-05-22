# Domain Taxonomy

## Domains

Assign articles to the domain that best fits the content's primary focus.

| Domain | Folder | What belongs here |
|--------|--------|------------------|
| Career | `wiki/career/` | Career advice, personal growth trajectories, compensation, job hunting, professional development, mentorship, management skills |
| Startup | `wiki/startup/` | Building products, startup marketing, positioning, community building, fundraising, hiring, strategy, go-to-market |
| Writing | `wiki/writing/` | Writing craft, content creation systems, newsletters, audience building, Twitter/social media writing, editing, publishing |
| Thinking | `wiki/thinking/` | Mental models, frameworks, life lessons, philosophy of learning, decision-making, wisdom, self-improvement principles |
| Productivity | `wiki/productivity/` | Workflows, habits, tools, note-taking systems, attention management, time management, planning systems |
| AI and Technology | `wiki/ai/` | LLMs, generative AI, machine learning, AI tools, prompt engineering, AI research, software systems |
| Creative | `wiki/creative/` | Drama, poetry, storytelling, fiction, creative process, craft analysis, literary criticism |
| Spiritual | `wiki/spiritual/` | Spiritual traditions, philosophy, meditation, contemplative practices, theology, consciousness |
| Research | `wiki/research/` | Academic papers, formal studies, technical deep-dives, methodology, data analysis |
| Bridges | `wiki/bridges/` | Cross-domain connection articles. Not a domain. See `templates/bridge-article.md`. |

## Classification Rules

**Primary domain wins.** An article about "hiring for startups" goes in `wiki/startup/` if the focus is building the company, or `wiki/career/` if the focus is career advice for the person being hired.

**When content is genuinely dual-domain,** pick the primary domain for the article and create a bridge article for the cross-domain insight. Do not put the article in both folders.

**Tag-to-domain mapping from Notion export:**

| Tag | Primary domain |
|-----|---------------|
| Career, Growth, Money | career/ |
| Startup, Product, Strategy, Community, Hiring | startup/ |
| Writing | writing/ |
| Thinking, Frameworks, Life Lesson | thinking/ |
| Productivity, Planning, Tools | productivity/ |
| Management | career/ or startup/ depending on context |
| Misc, Podcast | classify by content |

**When in doubt,** ask: "What question does this article answer?"
- "How do I advance my career?" -> career/
- "How do I build a company?" -> startup/
- "How do I write better?" -> writing/
- "How should I think about this?" -> thinking/
- "How do I get more done?" -> productivity/

## Creating New Domains

If 3 or more articles on a topic do not fit any existing domain, create a new folder in `wiki/`.

Rules:
- Name: short, lowercase, descriptive. One word preferred.
- Add a new section header to `wiki/_index.md`
- Do not over-fragment. Check if articles could fit an existing domain first.

## Domain-Specific Notes

**Career vs Startup:** These overlap. The test: is the advice for an individual navigating their career, or for someone building a company? Hiring frameworks written from the employer's perspective go in startup/. Career advice for the person being hired goes in career/.

**Writing vs Creative:** Writing/ covers the craft and systems of non-fiction content creation (newsletters, social posts, articles). Creative/ covers artistic work (poetry, drama, fiction, spiritual writing). A piece about "how to write viral threads" goes in writing/. A poem goes in creative/.

**Thinking vs Productivity:** Thinking/ covers mental models and principles. Productivity/ covers systems and tools. "Charlie Munger's mental compounding" is thinking/. "How to set up a note-taking system" is productivity/.

**Bridges:** Should grow steadily once content spans 3+ domains. If the wiki has 30+ articles across 3+ domains and zero bridges, run `/wiki connections`.
