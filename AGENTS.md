# KeywordSkills — Agent Instructions

This repository contains Agent Skills for [TheKeyword](https://thekeyword.io) keyword research API. Skills are installed to your agent's skills directory and provide procedural knowledge for keyword research workflows.

## Skills

| Skill | Purpose |
|-------|---------|
| `thekeyword-setup` | Foundation: API auth, base URL, geo/country config, plan tiers |
| `keyword-analysis` | Analyze keywords (single + batch with job polling) |
| `keyword-lookup` | Look up a specific keyword's full metrics and related data |
| `keyword-library` | Browse, filter, and sort all analyzed keywords |
| `keyword-lists` | Create lists and organize keywords into groups |

## Skill Dependencies

All skills depend on `thekeyword-setup`. Read it first for authentication and configuration.

```
thekeyword-setup (read first)
  ├── keyword-analysis
  ├── keyword-lookup
  ├── keyword-library
  └── keyword-lists
```

## Requirements

- A TheKeyword account ([thekeyword.io](https://thekeyword.io))
- An API key (`tk_live_...`) from [thekeyword.io/dashboard/api-keys](https://thekeyword.io/dashboard/api-keys)

## API Authentication

All API calls require a Bearer token:

```
Authorization: Bearer tk_live_your_key_here
```

API base URL: `https://api.thekeyword.io`

## Geo / Country

Always specify the target country when analyzing or querying keywords. Supported: US, GB, CA, AU, DE, FR, ES, IT, PT, BR, NL, BE, AT, CH, IE, NZ, IN, MX, AR, CO.

## Plan Awareness

Check `GET /api/v1/usage` before analyses. Each analysis costs 1 credit. Batch analysis and advanced features (forecasts, content briefs, discovery) require higher-tier plans.
