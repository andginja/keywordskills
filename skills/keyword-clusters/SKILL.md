---
name: keyword-clusters
description: "Manage keyword clusters — semantic groups of related keywords. Generate clusters, browse topics, drill into cluster details, and plan content silos. Use when the user asks about keyword grouping, topic clusters, content silos, or thematic keyword organization."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Clusters

Clusters group semantically similar keywords into topics using vector embeddings. Each cluster has aggregate metrics and an AI-generated label (Ultra plan).

## Prerequisites

- Read `thekeyword-setup` first for auth and configuration
- Requires **Pro+ plan** (Pro, Ultra, Enterprise)

## How It Works

1. Analyzed keywords are automatically embedded as vectors
2. Each analyzed keyword becomes a cluster seed
3. Similar keywords (≥80% cosine similarity) from the full database are pulled in
4. Small clusters merge; large ones split
5. Ultra users get AI-generated 2-4 word topic labels

Clusters generate **automatically** after keyword analysis when 10+ un-clustered keywords accumulate.

## Endpoints

### List Clusters

```
GET /api/v1/clusters?sort=-total_volume&limit=25
```

Sort options: `total_volume`, `keyword_count`, `opportunity_score`, `avg_difficulty`, `created_at`. Prefix `-` for descending.

Response fields per cluster: `id`, `label`, `ai_label`, `keyword_count`, `total_volume`, `avg_search_volume`, `avg_difficulty`, `avg_cpc`, `pct_rising`, `opportunity_score`, `top_keywords[]`.

### Get Cluster Detail

```
GET /api/v1/clusters/{id}?limit=50
```

Returns cluster metadata plus paginated keywords sorted by search volume.

### Generate Clusters

```
POST /api/v1/clusters/generate
Body: { "force": false }
```

- `force: false` — Incremental: only clusters new un-clustered keywords
- `force: true` — Full re-cluster: deletes existing clusters, starts fresh

Returns HTTP 202 — runs asynchronously.

## Workflows

### Browse clusters

1. `GET /api/v1/clusters?sort=-opportunity_score&limit=10`
2. Present each cluster with AI label, keyword count, volume, and opportunity score

### Plan content silos

1. List clusters by volume: `GET /api/v1/clusters?sort=-total_volume`
2. Get keywords for top clusters: `GET /api/v1/clusters/{id}?limit=50`
3. Within each cluster, identify:
   - **Pillar page**: highest-volume informational keyword
   - **Supporting content**: long-tail and commercial keywords
   - **Quick wins**: low difficulty + high opportunity

### Find best opportunity cluster

1. `GET /api/v1/clusters?sort=-opportunity_score&limit=5`
2. Drill into the top result: `GET /api/v1/clusters/{id}`
3. Analyze the keyword mix (intent, difficulty, trend)

## Related Skills

- **keyword-analysis** — Analyze keywords to feed the clustering pipeline
- **keyword-lists** — Organize cluster keywords into actionable lists
