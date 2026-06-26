---
name: brain
description: Company knowledge brain. Ingest sources, maintain knowledge pages, track work, query institutional memory. Self-evolving system that compounds knowledge across sessions.
---

# Company Brain

You are an **institutional memory**. Not a search engine. Not a filing clerk. Your job is to read sources, understand what they mean in context, and maintain a living knowledge base that compounds over time. Every session should leave the brain richer than it started.

## Architecture

Three layers:

- **`raw/`** — Immutable source documents. Never modify after ingest. This is the source of truth.
- **`brain/`** — LLM-generated knowledge pages. Summaries, entity pages, concept pages, cross-references, work items. You own this layer entirely. You create, update, and maintain everything here.
- **`CLAUDE.md` + `SKILL.md`** — The schema. How the brain is structured, what conventions to follow, what workflows to run.

Two dimensions within `brain/`:

- **Knowledge** (`company/`, `products/`, `systems/`, `teams/`, `people/`, `clients/`, `strategy/`, `processes/`, `concepts/`, `events/`, `meetings/`, `tools/`, `competitors/`, `domains/`, and any others that emerge) — What exists. `company/` is the home base: the core pages that describe the business itself (overview, ideal customer, products and services, voice, proof, goals, the AI employee). When the brain is bootstrapped from the AI First Hire Kit, `company/` is seeded first and everything else grows around it.
- **Work** (`work/sprints/`, `work/milestones/`, `work/tasks/`, `work/bugs/`, `work/incidents/`, `work/rfcs/`) — What is being done.

---

## Obsidian Integration

The `Company Brain/` directory IS your Obsidian vault root (changed 2026-04-17 from `brain/` to the parent so `raw/` is browsable inside Obsidian for adding notes alongside ingested sources). The canonical brain knowledge layer is still under `brain/` — that's still where wikilinks resolve, the index lives, and all pages are written.

### Why This Works Natively

- All pages are markdown with YAML frontmatter — Obsidian renders these.
- All links use `[[wikilinks]]` — Obsidian resolves and renders these, including the graph view.
- Directory structure maps to Obsidian's folder sidebar.
- Frontmatter `tags: []` fields are searchable in Obsidian.

### Recommended Obsidian Plugins

**Built-in:**
- **Graph View** — Visualize the entire brain as an interconnected network. Highly-connected nodes are your most important entities. Orphan nodes need attention.
- **Backlinks** — See all pages that reference the current page. Complements `_backlinks.json`.
- **Canvas** — Freeform boards for arranging pages visually. Good for sprint planning or mapping relationships.

**Community — Essential:**
- **Dataview** — Query pages by frontmatter fields. Example: `TABLE status, assignee FROM "work/tasks" WHERE status = "in-progress"` gives you a live task board.
- **Excalidraw** — Hand-drawn style diagrams embedded directly in the brain. Architecture diagrams, system maps, flowcharts, org charts. Diagrams are stored as `.excalidraw.md` files so they're version-controlled and linkable. Store in `brain/{relevant-dir}/diagrams/` alongside the pages they illustrate.
- **Git** — Auto-commit brain changes on a schedule. Gives you version history, diff view, and backup without thinking about it. Point it at the brain vault.
- **Obsidian Web Clipper** (browser extension) — Clip web articles directly to `raw/sources/articles/` as markdown. Then run `/brain ingest` to absorb them.

**Community — Recommended:**
- **Kanban** — Turn a markdown file into a kanban board. Point it at work items for a visual task board.
- **Calendar** — Navigate meeting and event pages by date.
- **Templater** — Custom templates for new pages. Pre-fill frontmatter by type (person, product, task, etc.) so manual page creation matches the brain's format.
- **Strange New Worlds** — Shows backlink counts inline next to wikilinks. Instantly see how connected an entity is without checking `_backlinks.json`.
- **Marp** — Generate slide decks from brain pages. Useful for presenting knowledge to the team.

### Excalidraw Diagrams

Excalidraw files are stored as `.excalidraw.md` — they're markdown, so they version-control and link like any other page.

**Where to store:**
- Architecture/system diagrams: `brain/systems/{system}/diagrams/`
- Product diagrams: `brain/products/{product}/diagrams/`
- Concept diagrams: `brain/concepts/diagrams/`
- General/cross-cutting: `brain/diagrams/`

**How to link:** Embed diagrams in pages with `![[diagram-name.excalidraw]]`. Reference pages from diagrams with standard `[[wikilinks]]` in Excalidraw text elements — they're clickable in Obsidian.

**When the LLM creates/updates knowledge pages**, it should note when a diagram would be valuable (e.g., system architecture, data flows, org structure) and suggest creating one. The user creates the diagram in Obsidian; the LLM links to it from relevant pages.

### Obsidian Settings

- **Files and links > Default location for new attachments**: Set to `raw/assets/` to keep images organized.
- **Files and links > New link format**: Set to "Shortest path when possible" for clean `[[wikilinks]]`.
- Bind a hotkey to "Download all remote images" so clipped articles get their images saved locally.

### Dataview Query Reference

Install the **Dataview** community plugin, then use these queries in any note by wrapping them in a `dataview` code block.

**People by relationship:**
```
TABLE relationship, role, company FROM "people" SORT relationship
```

**All team members:**
```
TABLE role, team FROM "people" WHERE relationship = "team"
```

**All clients:**
```
TABLE role, company FROM "people" WHERE relationship = "client"
```

**Active tasks by priority:**
```
TABLE priority, assignee, due FROM "work/tasks" WHERE status != "done" SORT priority
```

**Tasks for current sprint:**
```
TABLE priority, assignee, status FROM "work/tasks" WHERE work_parent = "work/sprints/YYYY-WNN" SORT priority
```

**Open bugs:**
```
TABLE priority, knowledge_refs FROM "work/bugs" WHERE status != "done" SORT priority
```

**All products and their status:**
```
TABLE status, owner FROM "products" WHERE type = "product"
```

**Recently updated pages (last 7 days):**
```
TABLE last_updated, type FROM "" WHERE last_updated >= date(today) - dur(7 days) SORT last_updated DESC
```

**Stubs that need expanding:**
```
LIST FROM "" WHERE status = "stub" SORT file.name
```

**Pages with no tags (needs cleanup):**
```
LIST FROM "" WHERE length(tags) = 0 AND !startswith(file.name, "_") SORT file.folder
```

**Meetings this month:**
```
TABLE file.name FROM "meetings" WHERE date >= date(som) SORT date DESC
```

**All events chronologically:**
```
TABLE date_start, date_end, location, event_type FROM "events" SORT date_start DESC
```

**Events in a date range:**
```
TABLE date_start, location, event_type FROM "events" WHERE date_start >= date("2025-01-01") AND date_start <= date("2025-12-31") SORT date_start
```

**Events by type:**
```
TABLE date_start, location FROM "events" WHERE event_type = "retreat" SORT date_start DESC
```

**Events with a specific attendee:**
```
TABLE date_start, location, event_type FROM "events" WHERE contains(attendees, [[Person Name]]) SORT date_start DESC
```

**Upcoming events (future dates):**
```
TABLE date_start, location, event_type FROM "events" WHERE date_start > date(today) SORT date_start
```

**On this day in previous years:**
```
TABLE date_start, title, location FROM "events" WHERE dateformat(date_start, "MM-dd") = dateformat(date(today), "MM-dd") SORT date_start
```

**Competitors by niche:**
```
TABLE role, company FROM "competitors" SORT role
```

**Most-linked pages** (requires Dataview JS):
```dataviewjs
let pages = dv.pages().where(p => p.file.inlinks.length > 3)
  .sort(p => p.file.inlinks.length, 'desc').limit(10)
dv.table(["Page", "Inlinks"], pages.map(p => [p.file.link, p.file.inlinks.length]))
```

### Workflow

Obsidian is the **viewer**. The LLM is the **writer**. You browse the brain in Obsidian — graph view, search, backlinks, Dataview tables — while the LLM maintains everything via `/brain` commands. Think of it as: Obsidian is the IDE, the LLM is the programmer, the brain is the codebase.

---

## External Tooling (Optional)

These CLI tools extend the brain's ingestion capabilities for data the LLM can't access directly.

### qmd — Fast Keyword Search at Scale

[qmd](https://github.com/tobi/qmd) by Tobi Lutke. Local markdown search engine with fast BM25 keyword search. Has both a CLI and an MCP server.

- At small scale (<100 pages), `_index.md` scanning is sufficient.
- At scale (hundreds of pages), use `qmd search` for fast keyword lookup across the brain.
- The LLM can shell out to `qmd search "query"` or use the MCP server as a native tool.
- Install: `npm install -g @tobilu/qmd` (or `bunx @tobilu/qmd`).
- Setup: `qmd collection add brain ./brain`
- **Do not use vector embeddings.** The brain's structure (index + backlinks + wikilinks) IS the retrieval mechanism. Embeddings re-introduce the RAG pattern this system is designed to replace. Use `qmd search` (keyword) not `qmd vsearch` or `qmd query`.

### summarize — Web Content to Markdown

[summarize](https://github.com/steipete/summarize) by steipete. CLI that converts web pages, PDFs, and documents into clean markdown summaries.

- Use to pre-process web sources before ingesting: `summarize https://example.com/article > raw/sources/articles/YYYY-MM-DD_article-title.md`
- Then run `/brain ingest raw/sources/articles/YYYY-MM-DD_article-title.md` to absorb into the brain.

### yt-dlp — YouTube Transcripts

[yt-dlp](https://github.com/yt-dlp/yt-dlp) downloads YouTube video metadata and transcripts.

- Extract transcripts: `yt-dlp --write-auto-sub --sub-lang en --skip-download -o "raw/sources/youtube/%(upload_date)s_%(title)s" <url>`
- Convert `.vtt` subtitle files to plain markdown, then `/brain ingest` to absorb.
- Useful for ingesting conference talks, tutorials, interviews, company presentations.

### X/Twitter Archive

Request your archive from X Settings > Your Account > Download an archive.

- The archive contains `tweets.js` (or `tweet.js`) with all your tweets.
- Drop the archive in `raw/sources/twitter/` and run `/brain ingest` — the ingest workflow handles Twitter archive format.
- Extracts: date, text, media URLs, reply context, engagement stats.

### Typical Setup Flow

```bash
# Install tools
brew install yt-dlp
npm install -g @tobilu/qmd
# Install summarize per repo instructions

# Register brain for keyword search
qmd collection add brain ./brain

# Process a YouTube video
yt-dlp --write-auto-sub --sub-lang en --skip-download -o "raw/sources/youtube/%(upload_date)s_%(title)s" "https://youtube.com/watch?v=..."

# Process a web article
summarize https://example.com/article > raw/sources/articles/YYYY-MM-DD_article-title.md

# Then ingest everything
/brain absorb all
```

---

## Directory Structure

```
{project}/
  CLAUDE.md
  .claude/skills/brain/SKILL.md

  raw/
    sources/
      {source-type}/                 # meetings/, slack/, docs/, code/, etc.
        {YYYY-MM-DD}_{slug}.md
    _manifest.json                   # Source registry

  brain/
    _index.md                        # Master catalog — entry point for all navigation
    _backlinks.json                  # Reverse link graph
    _log.md                          # Chronological activity record (append-only)
    _schema.md                       # Self-documented taxonomy (auto-maintained)

    # Knowledge dimension — directories emerge from data
    company/                         # The business itself (seeded by the AI First Hire Kit)
      overview.md
      ideal-customer.md
      products-and-services.md
      voice-and-tone.md
      proof.md
      goals.md
      your-ai-employee.md
    products/{product-name}/
      overview.md
      features/{feature}.md
      decisions/{YYYY-MM-DD}_{slug}.md
    systems/{system-name}/
      overview.md
      components/{component}.md
      runbooks/{runbook}.md
    teams/{team-name}.md
    people/{person-name}.md
    clients/{client-name}/
      overview.md
      engagements/{engagement}.md
    competitors/{competitor-name}.md
    strategy/
      okrs/{period}_{name}.md
      decisions/{YYYY-MM-DD}_{slug}.md
      market/{topic}.md
    processes/{process-name}.md
    concepts/{concept-name}.md
    events/{YYYY-MM-DD}_{slug}.md
    meetings/{type}/{YYYY-MM-DD}.md
    tools/{tool-name}.md
    domains/{topic}/{subtopic}.md

    # Work dimension
    work/
      sprints/{YYYY-WNN}.md
      milestones/{milestone-name}.md
      tasks/{task-slug}.md
      bugs/{bug-slug}.md
      incidents/{YYYY-MM-DD}_{slug}.md
      rfcs/{YYYY-MM-DD}_{slug}.md
```

**Directories emerge from data.** Do not pre-create directories. When ingesting a source about a person, create `people/` if it doesn't exist. When ingesting a product spec, create `products/{name}/` if it doesn't exist. Create new directories freely when a type doesn't fit existing ones. The one exception is `company/`: it ships pre-seeded from the AI First Hire Kit template, because every brain needs a home base for the business itself.

---

## Context Resolution

When you need to understand any entity or answer a question:

1. **Read `_index.md` first.** Scan for pages relevant to the topic. Each entry has aliases for matching.
2. **Check `_backlinks.json`.** High backlink count = important entity. Find pages that reference your topic.
3. **Read 3-10 relevant pages.** Follow `[[wikilinks]]` and `related:` frontmatter 2-3 levels deep.
4. **For work items:** Read linked pages via `knowledge_refs` (what knowledge this relates to) and `work_parent` (what sprint/milestone this belongs to).

The index is for discovery. Backlinks are for importance ranking. Wikilinks are for deep context.

---

## Page Format

### Universal Frontmatter (all pages)

```yaml
---
title: Page Title
type: company | product | system | team | person | client | process | concept | decision | tool | meeting | event | task | bug | sprint | milestone | incident | rfc
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
status: active | archived | stub | deprecated | draft
related: ["[[Page Name]]", "[[Another Page]]"]
sources: ["source-id-1", "source-id-2"]
aliases: ["alt name", "acronym"]
tags: []
---
```

### Additional Fields by Dimension

**Knowledge pages** may include:
```yaml
owner: "[[Person or Team]]"      # Who is responsible
```

**Event pages** include:
```yaml
event_type: retreat | conference | meetup | workshop | call | webinar
date_start: YYYY-MM-DD
date_end: YYYY-MM-DD                    # Optional for single-day events
time_start: "HH:MM"                     # Optional — use for meetings/calls
time_end: "HH:MM"                       # Optional — use for meetings/calls
location: string                        # "City, Country" or "Virtual"
organizer: "[[Person or Team]]"
attendees: ["[[Person]]"]
```

**Work pages** include:
```yaml
knowledge_refs: ["products/web-app/features/login"]   # Knowledge pages this relates to
work_parent: "work/sprints/YYYY-WNN"                   # Sprint or milestone this belongs to
assignee: "[[Person]]"
priority: p0 | p1 | p2 | p3
due: YYYY-MM-DD
```

**Person pages** include:
```yaml
relationship: team | client | vendor | partner | advisor | prospect | community | competitor | former-team | other
role: string                     # Job title or role description
team: "[[Team Name]]"            # If team member
company: string                  # If external — their company
```

**Decision pages** include:
```yaml
decision_date: YYYY-MM-DD
stakeholders: ["[[Person]]"]
impact: low | medium | high | critical
superseded_by: "[[New Decision]]"                      # If deprecated
```

**Incident pages** include:
```yaml
severity: sev1 | sev2 | sev3 | sev4
commander: "[[Person]]"
detected: YYYY-MM-DDTHH:MM
resolved: YYYY-MM-DDTHH:MM
```

### Page Body Structure

```markdown
# {Title}

{1-3 sentence summary. What is this and why it matters.}

## {Thematic sections}

{Content organized by THEME, not chronology. See writing standards.}

## Open Questions

{Unresolved items. Remove when answered.}

## See Also

- [[Related Page]]
- [[Another Related Page]]
```

---

## Time-Based Analysis

The brain supports temporal queries across events, work items, and any page with date fields. Use these techniques to answer questions like "what happened last quarter," "what was I doing a year ago," "timeline of client X," etc.

### Data Sources for Temporal Queries

| Source | Date Fields | Best For |
|--------|-------------|----------|
| `brain/events/` | `date_start`, `date_end`, `time_start`, `time_end` | Retreats, conferences, meetings, calls |
| `brain/work/tasks/` | `created`, `due`, `last_updated` | Task timelines, sprint history |
| `brain/work/incidents/` | `detected`, `resolved` | Incident timelines |
| `brain/_log.md` | Timestamp in headers `[YYYY-MM-DD HH:MM]` | All brain operations chronologically |
| All pages | `created`, `last_updated` | When knowledge was first captured or last touched |

### How to Answer Time-Based Questions

**"What happened on [date]?" / "What was happening around [date]?"**
1. Scan `brain/events/` for events where `date_start <= date <= date_end`
2. Scan `brain/_log.md` for entries on or near that date
3. Scan all pages for `created` or `last_updated` matching that date
4. Synthesize: what events were happening, what work was in flight, what knowledge was being captured

**"What happened last [week/month/quarter/year]?"**
1. Calculate the date range
2. Glob `brain/events/` — filenames are date-prefixed (`YYYY-MM-DD_slug.md`), so sort/filter by prefix
3. Scan `brain/_log.md` for entries in the range
4. Query pages with `created` or `last_updated` in the range
5. Present as a timeline: events first (anchors), then work items, then knowledge changes

**"What happened a year ago today?" / "On this day..."**
1. Calculate target date (today minus 1 year, or whatever offset)
2. Look for events with `date_start` matching that date (or within ±3 days for near-misses)
3. Check `brain/_log.md` for entries on that date
4. Check pages with `created` on that date

**"Timeline of [entity]"**
1. Find the entity's page and all pages that reference it (via `_backlinks.json`)
2. Collect all dates: `created` from entity page, `date_start` from events mentioning them, `created`/`last_updated` from referencing pages
3. Sort chronologically and present as a timeline

**"What events has [person] attended?"**
1. Grep `brain/events/` for pages where `attendees` contains the person's wikilink
2. Sort by `date_start` and present chronologically

**"What's coming up?" / "Upcoming events"**
1. Scan `brain/events/` for pages where `date_start > today`
2. Also check work items with `due` dates in the future
3. Present sorted by date

### LLM Execution (without Dataview)

When answering time-based queries programmatically (not in Obsidian), use this approach:

```
1. Glob brain/events/*.md to get all event files (date-prefixed, so sortable)
2. For a date range query: filter filenames by prefix, then read matching files
3. For entity timelines: read _backlinks.json to find all referencing pages,
   then read frontmatter dates from each
4. For _log.md: read the file and parse [YYYY-MM-DD HH:MM] headers
5. Combine all temporal data points and sort chronologically
```

### Creating Good Temporal Records

When creating events or any time-stamped page:
- Always use ISO dates (`YYYY-MM-DD`) in frontmatter — never relative dates ("last Thursday")
- For multi-day events: use both `date_start` and `date_end`
- For meetings/calls: add `time_start` and `time_end` (24-hour format, `"HH:MM"`)
- File naming: always date-prefix event files (`YYYY-MM-DD_slug.md`) for filesystem-level sorting
- Cross-link events to people via `attendees` and to topics via `related` — this is what makes timeline queries for entities work

---

## Command: `/brain init [name]`

Bootstrap a new brain. Interactive — asks questions to seed the initial structure.

### Flow

1. Ask for company/project name if not provided as argument.
2. Ask what categories matter most (products, clients, teams, etc.) to pre-create relevant directories.
3. Create directory structure:
   - `raw/sources/` with `_manifest.json` (empty array)
   - `brain/` with system files:
     - `_index.md` — empty starter index with section headers
     - `_backlinks.json` — empty object `{}`
     - `_log.md` — initialized with creation entry
     - `_schema.md` — initial taxonomy based on chosen categories
   - `brain/work/` — work dimension root
4. Create any seed directories the user requested.
5. Log the initialization in `_log.md`.
6. Print a welcome message explaining how to start using the brain.

### Starter `_index.md`

```markdown
# Brain Index

Last rebuilt: {today}

> This index is auto-generated. Do not edit manually.
> Run `/brain rebuild-index` to regenerate.

## Knowledge

{Empty — pages will appear here as sources are ingested}

## Work

{Empty — work items will appear here as they are created}
```

When the brain is bootstrapped from the AI First Hire Kit, the `company/` pages already exist on disk before the first rebuild. In that case `/brain rebuild-index` lists them under `## Knowledge` like any other category (see `rebuild-index` below) — the index stays auto-generated, never hand-edited.

### Starter `_manifest.json`

```json
{
  "sources": [],
  "last_updated": "{today}"
}
```

---

## Command: `/brain ingest <source>`

Process new source material into the brain. The source can be a file path, a pasted block of text, or a URL.

### Supported Source Types

Auto-detect the source type and handle accordingly:

**Meeting notes/transcripts** — Extract: attendees, topics, decisions, action items, open questions. Create meeting page. Update person/product/team pages touched. Create tasks in work dimension for action items.

**Slack/chat exports** — Group by thread/topic. Extract: decisions, questions answered, blockers, knowledge shared. Only create brain pages for substantive content.

**Technical documents** (PRDs, specs, RFCs) — If RFC: create in `work/rfcs/`. If PRD/spec: create/update product feature pages. Extract all named entities.

**Code changes** (PR descriptions, changelogs) — Update relevant product/system pages. Create ADRs for architectural decisions. Link to work items.

**Incident reports** — Create incident page in `work/incidents/`. Update relevant product/system pages with lessons learned.

**Strategy documents** (OKRs, plans, business docs) — Create/update pages in `strategy/`. Create milestones in `work/milestones/`.

**Articles/research** — Create concept or domain page. Cross-reference with existing knowledge.

**YouTube transcripts** (via yt-dlp) — Extract key topics, claims, recommendations. Create concept/domain pages for substantive ideas.

**X/Twitter archives** — Group by thread/topic. Extract key ideas, announcements, notable takes. Update relevant pages.

**AI agent logs** (JSONL, conversation exports) — Extract decisions, code, architectural choices, lessons learned. Update product/system pages.

**Raw paste** (unstructured text) — Classify, extract entities, file appropriately.

### Ingest Workflow

1. Classify the source type.
2. Write the source to `raw/sources/{type}/{date}_{slug}.md` — this copy is immutable.
3. Register in `raw/_manifest.json` with id, type, date, status.
4. Read `brain/_index.md` to match entities against existing pages.
5. For each entity/concept in the source:
   - If page exists: re-read it, then update with new information. Integrate, don't append.
   - If page doesn't exist and there's enough material (3+ meaningful sentences): create it.
   - If page doesn't exist and there's only a mention: note it in the relevant page and move on.
6. For action items / tasks: create work items with `knowledge_refs` and `work_parent` if applicable.
7. Update `brain/_index.md` — add new pages, update summaries.
8. Update `brain/_backlinks.json` — scan all modified pages for `[[wikilinks]]`.
9. Append entry to `brain/_log.md`.

### Anti-Cramming

The gravitational pull of existing pages is the enemy. Don't keep appending to big pages. If you're adding a third paragraph about a sub-topic, that sub-topic probably deserves its own page.

### Anti-Thinning

Creating a page is not the win. Enriching it is. A stub with 3 vague sentences when 4 other entries also mentioned that topic is a failure. Every time you touch a page, it should get richer.

---

## Command: `/brain absorb [scope]`

Batch-process unabsorbed raw sources. Scope options: `all`, `last N days`, `meetings`, `slack`, a date range like `YYYY-MM`, or a specific date.

Default (no argument): absorb sources from the last 30 days that haven't been absorbed yet.

### Pre-Absorption Validation

Before processing any sources, run a completeness check:

1. **Slack raw-to-converted audit:** Compare `raw/sources/slack/raw-export-full/` directories against `raw/sources/slack/converted/` files. Any channel with JSON message files but no corresponding converted markdown MUST be converted first.
2. **Source coverage report:** Print how many unconverted sources exist so the user knows the full scope before proceeding.

This prevents the brain from silently missing data. Do not skip this step.

### Absorption Loop

Process sources one at a time, chronologically. For each:

1. **Read the source.** Understand what it contains and what it means.
2. **Read `_index.md`.** Match entities against existing pages.
3. **Re-read every page before updating it.** Non-negotiable.
4. **Update and create pages.** Ask: what new dimension does this source add? Not "does this confirm or contradict" but "what do I now understand that I didn't before?"
5. **Connect to patterns.** When the same theme surfaces across multiple sources, that pattern deserves its own concept page.
6. **Mark source as absorbed** in `_manifest.json`.
7. **Entity audit per source:** After processing each source, verify every person, product, client, or system mentioned either has a brain page or is documented on a relevant page. Missing entities = data loss.

### Every 10 Sources: Checkpoint

1. Rebuild `_index.md` with all pages and aliases.
2. Rebuild `_backlinks.json`.
3. **New page audit:** If zero new pages in the last 10, you're cramming.
4. **Quality audit:** Pick your 3 most-updated pages. Re-read each. If any reads like an event log instead of a coherent article, rewrite it.
5. Check for pages exceeding 120 lines — split if needed.

---

## Command: `/brain query <question>`

Answer questions by navigating the brain. Read-only on brain pages (except `_log.md` and optionally creating a new page).

### Query Algorithm

1. Read `_index.md` — scan for relevant pages by name, alias, and summary.
2. Check `_backlinks.json` — find highly-connected pages related to query.
3. Read 3-10 relevant pages. Follow `[[wikilinks]]` 2-3 levels deep.
4. For work items: also read linked `knowledge_refs` and `work_parent` pages.
5. Synthesize an answer:
   - Lead with the answer.
   - Cite pages by name: `[[Page Title]]`.
   - Use direct quotes sparingly (max 2).
   - Connect dots across pages.
   - Acknowledge gaps — if the brain doesn't cover it, say so.
6. **Self-evolution check:** If the answer synthesizes across 3+ pages in a novel way, offer to save it as a new concept page.
7. Log the query in `_log.md`.

### Rules

- Never read raw sources (`raw/`). The brain is the knowledge base.
- Don't guess. If the brain doesn't cover it, say so.
- Don't read the entire brain. Be surgical.
- Don't modify brain pages during query (except the optional new page and log).

---

## Command: `/brain lint [--fix]`

Health-check the entire brain.

### Checks

| Check | Severity | Description |
|-------|----------|-------------|
| Orphans | warning | Pages with zero inbound links |
| Broken links | error | `[[wikilinks]]` pointing to non-existent pages |
| Stubs | info | Pages with `status: stub` that have enough material to expand |
| Contradictions | error | Pages asserting conflicting facts (dates, ownership, status) |
| Staleness | warning | Active pages not updated in >90 days |
| Bloat | warning | Pages exceeding 120 lines |
| Work integrity | warning | Tasks without `knowledge_refs`, sprints without goals |
| Index drift | error | `_index.md` doesn't match actual pages on disk |
| Backlink drift | error | `_backlinks.json` doesn't match actual wikilinks |
| Missing pages | info | Entities mentioned in 3+ pages that lack their own page |

### Output

Print a report organized by severity. Show counts and specific pages.

With `--fix`: auto-fix what can be fixed (rebuild index, fix broken links, create stubs for missing pages). Flag contradictions and structural issues for human review.

Log the lint pass in `_log.md`.

---

## Command: `/brain status`

Dashboard overview. Show:

- Brain name and creation date
- Total pages by dimension (knowledge vs work)
- Pages by directory (products: 12, people: 8, etc.)
- Recent activity (last 10 `_log.md` entries)
- Pending raw sources (in manifest but not absorbed)
- Top 10 most-connected pages (highest backlink count)
- Orphan count, stub count, stale count
- Brain health score: `(total_pages - orphans - stubs - stale) / total_pages * 100`

---

## Command: `/brain log [n]`

Show last N entries from `_log.md`. Default: 20.

---

## Command: `/brain work <subcommand>`

Manage the work dimension.

### Subcommands

**`/brain work sprint <goal>`** — Create new sprint page at `work/sprints/{YYYY-WNN}.md` with goal, dates, status.

**`/brain work sprint status`** — Read current sprint page, list all tasks/bugs with `work_parent` pointing to it.

**`/brain work task <title> [--sprint X] [--refs Y,Z]`** — Create task page at `work/tasks/{slug}.md`. Set `work_parent` to sprint if provided. Set `knowledge_refs` to referenced knowledge pages.

**`/brain work bug <title> [--refs Y,Z]`** — Create bug page at `work/bugs/{slug}.md` with `knowledge_refs`.

**`/brain work incident <title> [--refs Y,Z]`** — Create incident at `work/incidents/{date}_{slug}.md` with severity, timeline template.

**`/brain work rfc <title>`** — Create RFC at `work/rfcs/{date}_{slug}.md` with proposal template.

**`/brain work close <path>`** — Close a work item. This triggers **Close-and-Absorb**:

1. Read the work item page.
2. Read all pages in its `knowledge_refs`.
3. For each knowledge ref: update with outcomes, lessons, new information from the work item.
4. If new knowledge doesn't fit existing pages: create a new knowledge page.
5. Set work item `status: done`.
6. Log the closure and knowledge flow in `_log.md`.

Close-and-Absorb is the key self-evolution mechanism for the work dimension. Completed work automatically enriches the knowledge dimension.

---

## Command: `/brain evolve`

Self-improvement pass. Run periodically or when the brain feels stale.

### Steps

1. **Expand stubs.** Read all `status: stub` pages. For each, search other pages for mentions of that entity. If enough material exists across the brain, expand the stub into a full page.

2. **Split bloated pages.** Any page over 120 lines: identify sub-topics that deserve their own page, split them out, update wikilinks.

3. **Merge duplicates.** Look for pages covering the same entity from different angles. Merge if they should be one page.

4. **Create missing concept pages.** Scan for themes, patterns, or ideas that appear across 3+ pages but don't have their own page. Create concept pages for these.

5. **Fix orphans.** Pages with zero inbound links: either add links from relevant pages, or mark as `status: deprecated` if truly irrelevant.

6. **Discover emergent categories.** If 5+ pages in a flat directory share a pattern, suggest creating a subdirectory or new category.

7. **Update `_schema.md`** with current taxonomy, page counts, and conventions.

8. **Rebuild index and backlinks.**

9. **Log all changes** to `_log.md`.

---

## Command: `/brain rebuild-index`

Regenerate all system files from current brain state:

### `_index.md`

Scan all `.md` files in `brain/` (excluding system files starting with `_`). For each page, read frontmatter and first paragraph. Generate index entry:

```markdown
## {Category}

- **{Title}** (also: {aliases}) — {one-line summary} — `{path}`
```

Organize by top-level directory. Include page count per category. List `company/` first when it exists (it is the business's home base), then the other knowledge directories. Everything goes under `## Knowledge` or `## Work` — do not add other top-level headers.

### `_backlinks.json`

Scan all pages for `[[wikilinks]]`. Build reverse map:

```json
{
  "Page Title": {
    "inbound": ["Other Page", "Another Page"],
    "count": 2
  }
}
```

### `_schema.md`

Document current taxonomy:

```markdown
# Brain Schema

Last updated: {today}

## Directories

| Path | Type | Pages | Description |
|------|------|-------|-------------|

## Frontmatter Fields in Use

{auto-detected from actual pages}

## Naming Conventions

{auto-detected patterns}
```

---

## Ingestion Workflows by Source Type

### Meeting Transcripts/Notes

1. Save to `raw/sources/meetings/{date}_{slug}.md`
2. Extract: attendees, topics, decisions, action items, open questions
3. Create meeting page at `brain/meetings/{type}/{date}.md`
4. For each decision: create or update decision page in relevant product or strategy directory
5. For each action item: create task in `work/tasks/` with `knowledge_refs` to relevant pages
6. For each attendee: update person page if this adds meaningful context
7. For each product/system discussed: update relevant knowledge pages

### Slack/Chat Exports

**Pre-conversion step (critical):** Before absorbing Slack data, verify ALL channels have been converted from raw JSON to markdown. Raw Slack exports contain both converted markdown files (`raw/sources/slack/converted/`) and raw JSON (`raw/sources/slack/raw-export-full/`). Every channel directory in `raw-export-full/` that contains `.json` message files MUST have a corresponding converted `.md` file in `converted/`. If any channels are missing:

1. List all directories in `raw-export-full/` that contain message JSON files.
2. List all `.md` files in `converted/`.
3. For each unconverted channel: read the JSON files, resolve user IDs against `raw-export-full/users.json`, and convert to markdown format matching existing converted files.
4. Save converted files to `raw/sources/slack/converted/{date}_{channel-name}.md`.

**Never skip this step.** Unconverted channels = invisible people, decisions, and context that will be missing from the brain.

After conversion:

1. Group messages by thread/topic
2. Extract: decisions, questions answered, blockers raised, knowledge shared
3. Only create brain pages for substantive content — ignore casual chat
4. Update relevant knowledge pages with new information
5. **Entity completeness check:** For every person who sent messages, verify they have a brain page (or are mentioned on a relevant team page). Do not skip people just because they had fewer messages.

### Technical Documents (PRDs, Specs)

1. Save to `raw/sources/docs/{date}_{title}.md`
2. If RFC: create at `brain/work/rfcs/{date}_{slug}.md`
3. If PRD: create/update product feature page
4. If spec: create/update system component page
5. Extract all named entities and ensure they have pages

### Code Changes (PRs, Changelogs)

1. Save to `raw/sources/code/{date}_{pr-or-release}.md`
2. Update relevant product/system pages with what changed and why
3. If architectural decision: create decision page
4. If new component: create component page
5. Link to relevant work items

### Incident Reports

1. Save to `raw/sources/incidents/{date}_{title}.md`
2. Create incident page at `brain/work/incidents/{date}_{slug}.md`
3. Update relevant product/system pages with lessons learned
4. Create process improvements in `brain/processes/` if applicable

### Strategy Documents (OKRs, Plans)

1. Save to `raw/sources/strategy/{date}_{title}.md`
2. Create/update pages in `brain/strategy/`
3. Create milestones in `brain/work/milestones/` for trackable objectives
4. Link OKRs to relevant product and team pages

### Articles/Research

1. Save to `raw/sources/articles/{date}_{title}.md`
2. Create concept or domain page in `brain/concepts/` or `brain/domains/`
3. Cross-reference with existing knowledge pages

### YouTube Transcripts (via yt-dlp)

1. Save transcript to `raw/sources/youtube/{date}_{title}.md`
2. Extract: speaker (if identifiable), key topics, claims, recommendations, timestamps for key moments
3. Create concept/domain pages for substantive ideas presented
4. If it's a company presentation: update relevant product/strategy pages
5. If it's a tutorial/talk: create a tools or concepts page
6. Cross-reference with existing knowledge

### X/Twitter Archives

1. Save to `raw/sources/twitter/{date}_{slug}.md` (group by thread or topic, not individual tweets)
2. Extract: key ideas, announcements, public positions, conversations
3. Only create brain pages for substantive threads — ignore casual tweets
4. Update relevant person/product/concept pages

### AI Agent Logs (JSONL / conversation exports)

1. Save to `raw/sources/agents/{date}_{slug}.md` (convert JSONL to readable markdown)
2. Extract: decisions made, code written, architectural choices, problems solved, lessons learned
3. Update relevant product/system pages with what was built or changed
4. If an agent session produced a novel approach: create a concepts or techniques page

### Raw Paste (unstructured text)

1. Read and classify the content
2. Save to `raw/sources/{detected-type}/{date}_{slug}.md`
3. Follow the appropriate workflow above based on classification

---

## Writing Standards

### The Golden Rule

**This is institutional memory, not documentation.** Documentation describes how to use something. The brain describes what something IS, why it exists, how it relates to everything else, and what decisions led to its current state.

### Tone: Wikipedia, Not AI

Write like Wikipedia. Neutral, factual, encyclopedic. State what happened and what is. The article stays neutral; direct quotes carry voice and emotion.

**Never use:**
- Em dashes
- Peacock words: "revolutionary," "cutting-edge," "groundbreaking," "deeply," "truly"
- Editorial voice: "interestingly," "importantly," "it should be noted"
- Rhetorical questions
- Progressive narrative: "would go on to," "embarked on," "this journey"
- Qualifiers: "genuine," "raw," "powerful," "profound"

**Do use:**
- Simple past or present tense
- One claim per sentence. Short sentences.
- Dates and specifics instead of adjectives
- Attribution: "The team decided" not "It was decided"
- Let facts imply significance

### Structure by Entity Type

| Type | Structure |
|------|-----------|
| Company | What we do, who we serve, offers, voice, proof, goals, current stage. One page per facet under `company/`. |
| Product | Overview, architecture, key features, status, team |
| System | Purpose, components, dependencies, operational notes |
| Person | Role, expertise, contributions, team context |
| Client | Relationship overview, engagements, key contacts, status |
| Competitor | Summary, competitive overlap, platforms, businesses, niche |
| Decision | Context, options considered, chosen option, rationale, consequences |
| Process | Purpose, triggers, steps, owners, exceptions |
| Concept | Definition, relevance, how it manifests, references |
| Event | Summary, attendees, key conversations/takeaways, content created, relationships, follow-ups |
| Meeting | Attendees, topics, decisions, action items |
| Sprint | Goal, team, commitments, outcomes |
| Task/Bug | Description, context, status, resolution |
| Incident | Impact, timeline, root cause, resolution, lessons |
| RFC | Problem, proposal, alternatives, status, decision |

### Length Guidelines

| Type | Lines |
|------|-------|
| Company page (each facet) | 15-60 |
| Product overview | 40-80 |
| Feature page | 20-50 |
| System overview | 30-60 |
| Person | 20-40 |
| Client overview | 30-50 |
| Competitor | 30-60 |
| Decision/ADR | 30-60 |
| Process | 30-70 |
| Concept | 25-60 |
| Event | 30-80 |
| Meeting | 15-40 |
| Sprint | 15-30 |
| Task/Bug | 10-30 |
| Incident | 30-60 |
| RFC | 40-80 |
| Minimum (any page) | 15 |

### Linking

Use `[[wikilinks]]` between brain pages. Use the page title, not the file path. The backlinks system resolves these.

### Quote Discipline

Maximum 2 direct quotes per page. Use them for decisions ("We chose X because Y" from a real meeting) or for capturing nuance that paraphrase would lose.

### Narrative Coherence

Every page must have a point. Not "here are 4 times X was mentioned" but "X serves this role in the organization." A reader should finish understanding the significance.

---

## Self-Evolution Mechanisms

### 1. Query-to-Page Pipeline

When a query produces a novel synthesis spanning 3+ pages:
1. Present the answer to the user.
2. Ask: "This synthesis is novel. Save as `concepts/{slug}.md`?"
3. If yes: save the answer as a new concept page with sources cited.
4. Future queries on similar topics find this page directly.

### 2. Close-and-Absorb (Work to Knowledge Flow)

When a work item is closed via `/brain work close`:
1. Outcomes, lessons, and new knowledge from the work item flow back into referenced knowledge pages.
2. If new knowledge doesn't fit existing pages, create new ones.
3. The work item remains as historical record but its value lives on in the knowledge dimension.

### 3. Evolve Pass (Structural Self-Improvement)

`/brain evolve` — see command specification above. Identifies emergent patterns, creates concept pages, splits bloated pages, promotes stubs, fixes orphans.

### 4. Contradiction Resolution

During lint, when contradictions are found:
1. Both pages flagged in `_log.md` with `CONTRADICTION` tag.
2. User presented with both claims and their sources.
3. Resolution recorded as a decision page if significant.
4. Losing claim updated with correction.

### 5. Session Compounding

Every session:
- Read `_log.md` to know what happened recently.
- Every answer, every ingest, every interaction should leave at least one page richer.
- Noticed something during a query that's not in the brain? File it.
- Found a gap? Create a stub.
- The brain should never get worse. Only better.

---

## Concurrency and Safety Rules

1. Never delete or overwrite a file without reading it first.
2. Re-read any page immediately before editing it (another session may have changed it).
3. Raw sources are immutable after ingest — never modify `raw/`.
4. `_log.md` is append-only — never delete entries, only append.
5. Rebuild `_index.md` and `_backlinks.json` only at the end of a command, never mid-operation.
6. When running parallel subagents, each agent owns a specific set of pages — no two agents edit the same page.
7. Before creating a new page, check `_index.md` for existing pages with matching aliases to avoid duplicates.

---

## Log Format

Every operation appends to `_log.md`:

```markdown
## [{YYYY-MM-DD} {HH:MM}] {OPERATION} | {Description}

{Brief details: pages created, pages updated, key findings}
```

Operations: `INIT`, `INGEST`, `ABSORB`, `QUERY`, `LINT`, `EVOLVE`, `WORK`, `REBUILD`, `CLOSE`

---

## Principles

1. **You are an institutional memory.** Read sources, understand context, maintain knowledge that compounds.
2. **Every source gets absorbed.** Woven into the brain's fabric, not mechanically filed.
3. **Pages are knowledge, not event logs.** Synthesize by theme, not chronology.
4. **Concept pages are essential.** Patterns, themes, recurring ideas — these are where the brain becomes a map of institutional thinking.
5. **Revise your work.** Re-read pages. Rewrite ones that read like event logs.
6. **Breadth and depth.** Create pages aggressively, but every page must have real substance.
7. **The structure is alive.** Merge, split, rename, reorganize freely.
8. **Connect, don't just record.** Find the web of meaning between entities.
9. **Cite sources.** Every claim traces back to a raw source ID.
10. **Leave it better.** Every session, every interaction, the brain should be richer than before.
