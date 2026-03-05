---
name: keyword-analysis
description: "Analyze keywords using TheKeyword API. Triggers single keyword analysis or batch analysis with job polling. Use when the user says 'analyze keyword', 'research keyword', 'check volume for', 'keyword research', 'analyze these keywords', or 'batch analyze'. Always specify the target country."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Analysis

Analyze keywords to get search volume, difficulty, CPC, intent, trends, and more. Each analysis costs 1 credit.

> **Prerequisite:** Read `../thekeyword-setup/SKILL.md` for auth, API base URL, geo config, and plan limits.

## Before Analyzing

**Always check credits first:**

```
GET /api/v1/usage
```

If `credits_used >= credits_limit`, stop and inform the user. Do not attempt analysis without available credits.

**Always confirm the target country.** Ask the user if not specified. Include `country` in every analysis request.

## Single Keyword Analysis

Costs 1 credit. Returns full metrics immediately.

```
POST /api/v1/keywords/analyze
Content-Type: application/json

{
  "keyword": "remote work tools",
  "country": "US"
}
```

**Response includes:**
- `search_volume` — Monthly search volume
- `difficulty` / `difficulty_label` — SEO difficulty (0-100, easy/moderate/hard/very_hard)
- `cpc` / `cpc_currency` — Cost per click in local currency
- `competition` / `competition_level` — Advertiser competition (0-1, low/medium/high)
- `opportunity_score` — Overall opportunity rating (0-100)
- `intent` — Search intent (informational, commercial, transactional, navigational)
- `trend_direction` — Rising, stable, or declining
- `trend` — 12-month volume trend (array, oldest first)
- `signals` — Market signals (Starter+ plan)
- `forecast_volume_30d` / `forecast_volume_90d` — Volume forecasts (Pro+ plan)

## Batch Analysis

Analyzes multiple keywords asynchronously. Returns a job ID to poll. Each keyword costs 1 credit.

**Plan limits on batch size:**
- **Free:** Batch not available
- **Starter:** Up to 10 keywords per batch
- **Pro:** Up to 50 keywords per batch
- **Ultra:** Up to 100 keywords per batch

```
POST /api/v1/keywords/batch
Content-Type: application/json

{
  "keywords": ["remote work tools", "hybrid office software", "digital nomad gear"]
}
```

**Response (HTTP 202):**

```json
{
  "object": "job",
  "id": "job_abc123",
  "status": "pending",
  "progress": { "completed": 0, "total": 3 }
}
```

### Polling for Results

Poll the job endpoint until `status` is `completed` or `failed`:

```
GET /api/v1/jobs/{jobId}
```

**Status transitions:** `pending` -> `processing` -> `completed` | `failed`

Poll every 3-5 seconds. When `completed`, the `result` field contains all analyzed keywords.

If `status` is `failed`, check the `error` field and inform the user.

## Workflow

### Single keyword

1. `GET /api/v1/usage` — Check credits
2. Confirm country with user
3. `POST /api/v1/keywords/analyze` — Analyze
4. Present results: volume, difficulty, CPC, intent, trend

### Batch keywords

1. `GET /api/v1/usage` — Check credits (need 1 per keyword)
2. Confirm country with user
3. Verify batch size fits plan limit
4. `POST /api/v1/keywords/batch` — Submit batch
5. `GET /api/v1/jobs/{jobId}` — Poll until complete
6. Present results summary: table of keywords with key metrics

## Examples

**User:** "Analyze the keyword 'best crm software' for the US market"

1. Check credits: `GET /api/v1/usage`
2. Analyze: `POST /api/v1/keywords/analyze` with `{"keyword": "best crm software", "country": "US"}`
3. Present: volume, difficulty, CPC, intent, opportunity score

**User:** "Research these keywords for Portugal: crm software, crm tools, crm comparison"

1. Check credits: `GET /api/v1/usage` — need 3 credits
2. Batch: `POST /api/v1/keywords/batch` with `{"keywords": ["crm software", "crm tools", "crm comparison"]}`
3. Poll: `GET /api/v1/jobs/{jobId}` until complete
4. Present: comparison table sorted by volume

## Library Batch Analysis

You can pick unanalyzed keywords from the user's library (keywords discovered as related results but not yet fully analyzed) and batch-analyze them. This combines the **keyword-library** and **batch analysis** workflows.

### Workflow

1. `GET /api/v1/usage` — Check credits
2. `GET /api/v1/keywords?status=known&sort=-search_volume&limit=20` — Fetch top unanalyzed keywords
3. Present the list to the user and let them confirm which to analyze
4. Extract the `keyword` field from each result
5. `POST /api/v1/keywords/batch` — Submit as batch with `{"keywords": ["keyword1", "keyword2", ...]}`
6. Poll `GET /api/v1/jobs/{jobId}` until complete

### Available Filters for Unanalyzed Keywords

Use any filter from the keyword-library skill to narrow down which unanalyzed keywords to analyze:

```
GET /api/v1/keywords?status=known&sort=-search_volume&limit=10
GET /api/v1/keywords?status=known&sort=-cpc&difficulty_lte=30&limit=20
GET /api/v1/keywords?status=known&sort=-opportunity_score&search_volume_gte=1000&limit=15
GET /api/v1/keywords?status=known&intent=commercial&sort=-search_volume&limit=10
```

### Example

**User:** "Analyze the top 10 highest-volume unanalyzed keywords"

1. Check credits: `GET /api/v1/usage` — need 10 credits
2. Fetch: `GET /api/v1/keywords?status=known&sort=-search_volume&limit=10`
3. Show user the preview list with volumes
4. On confirmation, extract keyword names and submit batch
5. Poll until complete, present results

## Tips

- Keyword slugs are URL-safe: "remote work tools" becomes "remote-work-tools"
- After analysis, use the **keyword-lookup** skill to get detailed data including related keywords and SERP analysis
- Use the **keyword-lists** skill to organize analyzed keywords into groups
- Batch analysis is async — always poll the job, don't assume instant results
- Use `status=known` to find unanalyzed keywords discovered from related results

## Related Skills

- **thekeyword-setup** — API authentication and configuration
- **keyword-lookup** — Deep-dive into a specific keyword
- **keyword-library** — Browse all analyzed keywords
- **keyword-lists** — Organize keywords into lists
