---
name: keyword-discovery
description: "Discover new keyword opportunities — explore topics, find content gaps, and generate keyword ideas from seeds. Use when the user asks about finding new keywords, topic exploration, content gaps, or keyword ideation."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Discovery

Find new keyword opportunities through topic exploration, semantic gap analysis, and seed-based idea generation.

## Prerequisites

- Read `thekeyword-setup` first for auth and configuration
- Discovery features require **Pro+ plan**
- Gap analysis requires existing analyzed keywords with embeddings

## Endpoints

### Explore a Topic

```
POST /api/v1/discover/explore
Body: { "topic": "email marketing automation", "geo": "US" }
```

Uses AI to explore a topic and find keyword opportunities across different intents and angles. Returns keywords with metrics.

### Find Keyword Gaps

```
POST /api/v1/discover/gaps
Body: { "seeds": ["crm software", "sales automation"], "limit": 20 }
```

Semantic search: finds keywords similar to your seeds that you haven't analyzed yet. Great for expanding coverage in a topic area.

### Generate Ideas from Seeds

```
GET /api/v1/discover/ideas?seeds=crm,sales+automation&limit=50
```

Uses Google Ads Keyword Planner to generate keyword ideas from seed terms. Returns volume, CPC, and competition data.

### Content Briefs

```
POST /api/v1/keywords/{slug}/brief    # Generate AI brief (Pro+)
GET  /api/v1/keywords/{slug}/brief    # Get existing brief
```

AI-generated content brief based on SERP analysis, keyword metrics, and competitive data. Includes suggested outline, word count, and key topics to cover.

## Workflows

### Topic expansion

1. Start with a seed topic: `POST /api/v1/discover/explore`
2. Batch analyze the best opportunities: `POST /api/v1/keywords/batch`
3. Find gaps you missed: `POST /api/v1/discover/gaps`
4. Organize into clusters — they generate automatically after 10+ analyzed keywords

### Content planning

1. Pick a high-opportunity keyword from your library
2. Generate a content brief: `POST /api/v1/keywords/{slug}/brief`
3. Get PAA questions: `GET /api/v1/serp/{slug}/questions`
4. Get discussion insights: `GET /api/v1/serp/{slug}/discussions`
5. Use the brief + questions + discussions to outline content

### Competitor keyword mining

1. Get domain dominance: `GET /api/v1/serp/domains`
2. Identify competitor domains
3. Find gaps in their coverage: `POST /api/v1/discover/gaps`
4. Explore adjacent topics: `POST /api/v1/discover/explore`

## Related Skills

- **keyword-analysis** — Analyze discovered keywords
- **keyword-clusters** — See how new keywords cluster with existing ones
- **keyword-serp** — Deep-dive into SERP for discovered keywords
- **keyword-lists** — Organize discoveries into lists
