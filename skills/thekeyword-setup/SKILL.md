---
name: thekeyword-setup
description: "Foundation skill for TheKeyword API. Sets up authentication, API base URL, and geo/country configuration. Read this skill first before using any other TheKeyword skill. Use when the user mentions 'TheKeyword', 'keyword API', 'API key setup', or 'configure TheKeyword'."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# TheKeyword Setup

This skill configures access to [TheKeyword](https://thekeyword.io) API. **All other TheKeyword skills depend on this one** — read it first.

## Authentication

Every request requires a Bearer token. Get your API key at [thekeyword.io/dashboard/api-keys](https://thekeyword.io/dashboard/api-keys).

API keys look like `tk_live_...` (production) or `tk_test_...` (testing).

**HTTP requests:**

```
Authorization: Bearer tk_live_your_key_here
```

**MCP server (recommended for Claude Code):**

If the `@thekeyword/mcp` server is connected, use the `thekeyword:execute` tool instead of raw HTTP. Authentication is handled automatically — never include API keys in your code.

```json
{
  "mcpServers": {
    "thekeyword": {
      "command": "npx",
      "args": ["@thekeyword/mcp"],
      "env": {
        "THEKEYWORD_API_KEY": "tk_live_..."
      }
    }
  }
}
```

## API Base URL

```
https://api.thekeyword.io
```

All endpoints are prefixed with `/api/v1/`.

## Country / Geo Configuration

TheKeyword supports multi-country keyword analysis. Always specify the `country` or `geo` parameter when analyzing or querying keywords.

**Supported countries:** US, GB, CA, AU, DE, FR, ES, IT, PT, BR, NL, BE, AT, CH, IE, NZ, IN, MX, AR, CO

**Check the user's default country:**

```
GET /api/v1/settings
```

Returns `country` field with the user's default. If the user hasn't specified a country, ask them which market they're targeting.

**Update default country:**

```
PATCH /api/v1/settings
Content-Type: application/json

{ "country": "PT" }
```

## Plan Tiers & Limits

Always check credits before running analyses. Different plans have different capabilities:

| Feature | Free | Starter | Pro | Ultra |
|---------|------|---------|-----|-------|
| Monthly credits | 3 | 200 | 1,000 | 5,000 |
| Batch analysis | -- | 10/batch | 50/batch | 100/batch |
| Lists | 1 | 10 | 50 | 200 |
| API keys | 1 | 3 | 5 | 10 |
| Signals | -- | Yes | Yes | Yes |
| Forecasts | -- | -- | Yes | Yes |
| Content briefs | -- | -- | Yes | Yes |
| Discovery | -- | -- | Yes | Yes |
| Clusters | -- | -- | Yes | Yes |

**Check usage before any analysis:**

```
GET /api/v1/usage
```

Returns:
- `credits_used` / `credits_limit` — monthly credit consumption
- `rate_limit.hourly_remaining` — requests left this hour

If `credits_used >= credits_limit`, inform the user they're out of credits and suggest upgrading at [thekeyword.io/dashboard](https://thekeyword.io/dashboard).

## Error Handling

All errors follow RFC 7807:

```json
{ "status": 429, "title": "Rate Limit Exceeded", "detail": "..." }
```

| Status | Meaning | Action |
|--------|---------|--------|
| 401 | Invalid or missing API key | Check Bearer token |
| 402 | Insufficient credits | Check `/api/v1/usage`, suggest upgrade |
| 403 | Feature not available on plan | Inform user which plan is needed |
| 429 | Rate limited | Wait and retry after `Retry-After` header |
| 404 | Resource not found | Check slug/ID |

## Verifying Setup

Test that everything works:

```
GET /api/v1/usage
```

A successful response confirms your API key is valid and shows your current plan.

## Related Skills

- **keyword-analysis** — Analyze keywords (single + batch)
- **keyword-lookup** — Check a specific keyword's data
- **keyword-library** — Browse all your analyzed keywords
- **keyword-lists** — Organize keywords into named lists
- **keyword-clusters** — Semantic keyword grouping and topic clusters (Pro+)
- **keyword-signals** — Market change alerts and opportunities (Starter+)
- **keyword-serp** — SERP analysis, domain dominance, PAA questions
- **keyword-discovery** — Topic exploration, gap analysis, content briefs (Pro+)
