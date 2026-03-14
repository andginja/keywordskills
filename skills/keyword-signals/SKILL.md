---
name: keyword-signals
description: "Retrieve and interpret keyword signals — actionable alerts about market changes. Includes breakouts, volume spikes/drops, featured snippet opportunities, seasonal peaks, new competitors, and difficulty drops. Use when the user asks about keyword alerts, market signals, opportunities, or changes."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Signals

Signals are automatically detected alerts about meaningful changes in your keyword landscape.

## Prerequisites

- Read `thekeyword-setup` first for auth and configuration
- Requires **Starter+ plan**

## Signal Types

| Signal | Meaning | Priority |
|--------|---------|----------|
| `breakout` | Massive volume surge (>200%) | Act now |
| `volume_spike` | Significant volume increase | Act this week |
| `volume_decline` | Noticeable volume decrease | Investigate |
| `rising_trend` | Consistent upward trend | Monitor |
| `seasonal_peak` | Approaching seasonal high | Prepare content |
| `difficulty_drop` | SEO difficulty decreased | Quick win |
| `featured_snippet_opportunity` | Snippet you could target | Act now |
| `new_competitor` | New domain in top SERPs | Investigate |

## Endpoints

### Get Signals

```
GET /api/v1/trends/signals?limit=50
```

Filters:
- `type=breakout` — Filter by signal type
- `severity=high` — Filter by severity (low, medium, high)
- `geo=US` — Filter by country

Each signal includes: `type`, `value`, `severity`, `keyword`, `keyword_slug`, `search_volume`, `difficulty`.

### Related Trend Endpoints

```
GET /api/v1/trends/rising?limit=20       # Rising keywords
GET /api/v1/trends/opportunities?limit=20 # High-opportunity keywords
GET /api/v1/trends/declining?limit=20     # Declining keywords
```

## Workflows

### Morning briefing

1. `GET /api/v1/trends/signals?severity=high&limit=10`
2. Present high-severity signals with recommended actions
3. Follow up with medium-severity if relevant

### Find quick wins

1. `GET /api/v1/trends/signals?type=difficulty_drop&limit=20`
2. Cross-reference with keyword details for full context

### Seasonal planning

1. `GET /api/v1/trends/signals?type=seasonal_peak`
2. Check keyword seasonality curves via `GET /api/v1/keywords/{slug}`
3. Plan content to publish before the peak

### Competitive monitoring

1. `GET /api/v1/trends/signals?type=new_competitor`
2. For interesting signals, check full SERP: `GET /api/v1/serp/{slug}`

## Related Skills

- **keyword-lookup** — Get full details on a signaled keyword
- **keyword-serp** — Deep-dive into SERP changes
