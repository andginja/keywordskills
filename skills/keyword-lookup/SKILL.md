---
name: keyword-lookup
description: "Look up a specific keyword's full data from TheKeyword API. Returns search volume, difficulty, CPC, intent, trends, related keywords, SERP data, and content briefs. Use when the user says 'check keyword', 'show me data for', 'keyword details', 'what's the volume for', 'keyword metrics', or 'look up keyword'."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Lookup

Get full details for a specific keyword that has already been analyzed.

> **Prerequisite:** Read `../thekeyword-setup/SKILL.md` for auth, API base URL, and geo config.

## Get Keyword Details

```
GET /api/v1/keywords/{slug}
```

The `slug` is the URL-safe version of the keyword: "remote work tools" becomes `remote-work-tools`.

**Optional query parameters:**
- `geo=PT` — Country code (shows data for that market)
- `include=related,serp,brief` — Expand inline (comma-separated)

### Expansions

Use `?include=` to fetch related data in one call instead of multiple requests:

| Expansion | Description | Plan required |
|-----------|-------------|---------------|
| `related` | Related/similar keywords | All plans |
| `serp` | SERP analysis (organic results, features, questions) | All plans |
| `brief` | AI-generated content brief | Pro+ |

**Example with all expansions:**

```
GET /api/v1/keywords/remote-work-tools?geo=US&include=related,serp,brief
```

## Response Fields

| Field | Description |
|-------|-------------|
| `search_volume` | Monthly search volume |
| `difficulty` | SEO difficulty score (0-100) |
| `difficulty_label` | easy, moderate, hard, very_hard |
| `cpc` | Cost per click (local currency) |
| `cpc_currency` | Currency code (USD, EUR, GBP, etc.) |
| `competition` | Advertiser competition (0-1) |
| `competition_level` | low, medium, high |
| `opportunity_score` | Opportunity rating (0-100) |
| `intent` | informational, commercial, transactional, navigational |
| `trend_direction` | rising, stable, declining |
| `trend` | 12-month volume array (oldest first) |
| `seasonality` | Monthly seasonality indices |
| `signals` | Market signals (Starter+) |
| `forecast_volume_30d` | 30-day volume forecast (Pro+) |
| `forecast_volume_90d` | 90-day volume forecast (Pro+) |
| `forecast_direction_30d` | Forecast direction (Pro+) |
| `forecast_confidence` | Forecast confidence score (Pro+) |

## Related Endpoints

**Get related keywords separately:**

```
GET /api/v1/keywords/{slug}/related?geo=US
```

**Get historical metrics:**

```
GET /api/v1/keywords/{slug}/history?geo=US
```

Returns historical snapshots of volume, CPC, and competition over time.

**Get SERP analysis separately:**

```
GET /api/v1/serp/{slug}?geo=US
```

Returns organic results, SERP features (featured snippets, PAA, knowledge panels), related questions, discussions, videos, and news.

**Get or generate content brief:**

```
GET /api/v1/keywords/{slug}/brief
POST /api/v1/keywords/{slug}/brief
```

GET returns existing brief. POST generates a new AI content brief (Pro+ plan, uses GPT based on SERP and keyword data).

## Check if a Keyword Exists

Before looking up, you can check if keywords are already in the database:

```
POST /api/v1/keywords/check-slugs
Content-Type: application/json

{
  "slugs": ["remote-work-tools", "hybrid-office"]
}
```

Returns a map of slug to status (`analyzed` or `known`). If a keyword doesn't exist, use the **keyword-analysis** skill to analyze it first.

## Workflow

1. Convert the keyword to a slug (lowercase, hyphens for spaces)
2. `GET /api/v1/keywords/{slug}?include=related,serp` — Fetch data with expansions
3. If 404: keyword hasn't been analyzed yet — offer to analyze it using keyword-analysis skill
4. Present the data: volume, difficulty, CPC, intent, trend, opportunity score
5. If the user wants more detail: show related keywords, SERP breakdown, or generate content brief

## Examples

**User:** "Show me data for 'email marketing' in the US"

```
GET /api/v1/keywords/email-marketing?geo=US&include=related,serp
```

Present: volume, difficulty, CPC, intent, trend direction, top related keywords, SERP features present.

**User:** "What's the search volume for 'crm software' in Portugal?"

```
GET /api/v1/keywords/crm-software?geo=PT
```

Present the volume and note the CPC is in EUR for the PT market.

**User:** "Compare 'project management' across US and UK"

```
GET /api/v1/keywords/project-management?geo=US
GET /api/v1/keywords/project-management?geo=GB
```

Present side-by-side comparison of volume, difficulty, CPC (note different currencies).

## Tips

- Always include `?geo=XX` to get country-specific data
- Use `?include=related` to discover more keyword ideas in one call
- If a keyword returns 404, it needs to be analyzed first — don't retry, suggest analysis
- CPC currency varies by country (US=USD, PT=EUR, GB=GBP, etc.)

## Related Skills

- **thekeyword-setup** — API authentication and configuration
- **keyword-analysis** — Analyze new keywords
- **keyword-library** — Browse all keywords
- **keyword-lists** — Organize into lists
