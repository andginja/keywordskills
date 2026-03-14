---
name: keyword-serp
description: "Analyze SERP (Search Engine Results Page) data for keywords. Get organic results, featured snippets, People Also Ask questions, discussion forums, and domain dominance analysis. Use when the user asks about search results, SERP features, competitor rankings, or domain visibility."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# SERP Analysis

Analyze search engine results pages for your keywords — organic rankings, SERP features, questions, discussions, and competitive domain analysis.

## Prerequisites

- Read `thekeyword-setup` first for auth and configuration
- Keywords must be analyzed first (use `keyword-analysis` skill)

## Endpoints

### SERP Snapshot

```
GET /api/v1/serp/{slug}?geo=US
```

Returns: organic results (position, URL, title, snippet), SERP features (featured snippet, knowledge panel, etc.), videos, news, and image packs.

### People Also Ask

```
GET /api/v1/serp/{slug}/questions
```

Returns PAA questions — useful for FAQ sections, content ideation, and understanding searcher intent.

### Discussion Results

```
GET /api/v1/serp/{slug}/discussions
```

Returns Reddit threads, forum posts, and Quora answers appearing in SERPs — shows what real users ask about.

### Domain Dominance

```
GET /api/v1/serp/domains?geo=US&limit=25
```

Aggregates SERP data across all your analyzed keywords. Shows which domains appear most frequently with average position, visibility scores, and keyword coverage.

**Requires Pro+ plan.**

## Workflows

### Competitive analysis

1. Get domain dominance: `GET /api/v1/serp/domains?limit=20`
2. Identify top competitors by visibility and keyword count
3. For specific keywords, get full SERP: `GET /api/v1/serp/{slug}`

### Content gap analysis

1. Get SERP for target keyword: `GET /api/v1/serp/{slug}`
2. Get PAA questions: `GET /api/v1/serp/{slug}/questions`
3. Get discussions: `GET /api/v1/serp/{slug}/discussions`
4. Cross-reference with existing content to find gaps

### Featured snippet targeting

1. Check signals for snippet opportunities: `GET /api/v1/trends/signals?type=featured_snippet_opportunity`
2. Get SERP snapshot to see current snippet holder
3. Analyze snippet format (paragraph, list, table) to optimize your content

## Related Skills

- **keyword-analysis** — Analyze keywords to generate SERP snapshots
- **keyword-signals** — Get alerts for SERP changes
- **keyword-discovery** — Find new keywords from SERP insights
