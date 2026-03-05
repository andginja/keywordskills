---
name: keyword-library
description: "Browse and filter all analyzed keywords in TheKeyword. Supports sorting by volume, difficulty, CPC, filtering by country, intent, trend direction, and text search. Use when the user says 'show my keywords', 'list keywords', 'keyword library', 'browse keywords', 'my analyzed keywords', 'top keywords', or 'highest volume keywords'."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Library

Browse, filter, and sort all keywords that have been analyzed.

> **Prerequisite:** Read `../thekeyword-setup/SKILL.md` for auth, API base URL, and geo config.

## List Keywords

```
GET /api/v1/keywords
```

### Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 25 | Results per page (max 100) |
| `cursor` | string | -- | Pagination cursor from previous response |
| `geo` | string | -- | Filter by country code (US, GB, PT, etc.). Omit for all countries. |
| `status` | string | -- | `analyzed` or `known` |
| `sort` | string | `-updated_at` | Sort field. Prefix `-` for descending. |
| `q` | string | -- | Text search across keyword names |
| `trend_direction` | string | -- | `rising`, `stable`, or `declining` |
| `difficulty_gte` | integer | -- | Minimum difficulty (0-100) |
| `difficulty_lte` | integer | -- | Maximum difficulty (0-100) |
| `search_volume_gte` | integer | -- | Minimum search volume |
| `intent` | string | -- | `informational`, `commercial`, `transactional`, `navigational` |

### Sort Options

| Sort value | Description |
|------------|-------------|
| `-search_volume` | Highest volume first |
| `search_volume` | Lowest volume first |
| `-difficulty` | Hardest first |
| `difficulty` | Easiest first |
| `-cpc` | Highest CPC first |
| `-opportunity_score` | Best opportunities first |
| `-updated_at` | Most recently updated first |

### Response Format

```json
{
  "object": "list",
  "data": [
    {
      "object": "keyword_summary",
      "id": "kw_abc123",
      "keyword": "remote work tools",
      "slug": "remote-work-tools",
      "geo": "US",
      "search_volume": 12000,
      "cpc": 4.50,
      "difficulty": 45,
      "intent": "commercial",
      "trend_direction": "rising",
      "analysis_status": "analyzed"
    }
  ],
  "has_more": true,
  "next_cursor": "eyJ...",
  "total": 150
}
```

### Pagination

Use cursor-based pagination. If `has_more` is `true`, pass `next_cursor` as the `cursor` parameter to get the next page:

```
GET /api/v1/keywords?cursor=eyJ...&limit=25
```

## Common Queries

**Top keywords by volume:**

```
GET /api/v1/keywords?sort=-search_volume&limit=10
```

**Easy, high-volume opportunities:**

```
GET /api/v1/keywords?sort=-opportunity_score&difficulty_lte=40&search_volume_gte=1000&limit=10
```

**Rising keywords in Portugal:**

```
GET /api/v1/keywords?geo=PT&trend_direction=rising&sort=-search_volume
```

**Search for specific terms:**

```
GET /api/v1/keywords?q=email+marketing&sort=-search_volume
```

**Commercial intent keywords with high CPC:**

```
GET /api/v1/keywords?intent=commercial&sort=-cpc&limit=20
```

**All keywords across all countries:**

```
GET /api/v1/keywords?limit=100
```

## Workflow

1. Ask the user what they want to see (or infer from context)
2. Build the query with appropriate filters and sort
3. `GET /api/v1/keywords?...` — Fetch results
4. Present as a table: keyword, volume, difficulty, CPC, intent, trend
5. If `has_more` is true, offer to load more results
6. Offer next steps: look up details (keyword-lookup), organize into lists (keyword-lists)

## Examples

**User:** "Show me my top 10 keywords by volume"

```
GET /api/v1/keywords?sort=-search_volume&limit=10
```

Present as a ranked table with volume, difficulty, and trend.

**User:** "Find easy keywords with decent volume"

```
GET /api/v1/keywords?difficulty_lte=30&search_volume_gte=500&sort=-opportunity_score&limit=20
```

**User:** "What rising keywords do I have?"

```
GET /api/v1/keywords?trend_direction=rising&sort=-search_volume
```

**User:** "Show all my Portugal keywords"

```
GET /api/v1/keywords?geo=PT&sort=-search_volume
```

## Unanalyzed Keywords

Keywords with `status=known` are discovered as related results but not yet fully analyzed (no signals, forecasts, or content briefs). Use them to find high-potential keywords to analyze next:

```
GET /api/v1/keywords?status=known&sort=-search_volume&limit=20
GET /api/v1/keywords?status=known&sort=-opportunity_score&difficulty_lte=30&limit=10
```

To batch-analyze them, see the **Library Batch Analysis** section in the **keyword-analysis** skill.

## Tips

- Omitting `geo` returns keywords from all countries — useful for a global overview
- Combine multiple filters for precise results (e.g., `intent=commercial&difficulty_lte=50&geo=US`)
- The `opportunity_score` is a composite metric factoring in volume, difficulty, and competition — sort by it to find quick wins
- Use `q=` for text search when you know part of a keyword name

## Related Skills

- **thekeyword-setup** — API authentication and configuration
- **keyword-analysis** — Analyze new keywords
- **keyword-lookup** — Deep-dive into a specific keyword
- **keyword-lists** — Organize keywords into lists
