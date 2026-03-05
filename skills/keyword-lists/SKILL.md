---
name: keyword-lists
description: "Create and manage keyword lists in TheKeyword. Organize analyzed keywords into named groups for content planning, campaigns, or topic clusters. Use when the user says 'create a list', 'add to list', 'organize keywords', 'keyword list', 'show my lists', 'group these keywords', or 'content plan'."
metadata:
  author: TheKeyword
  version: "1.0.0"
---

# Keyword Lists

Organize analyzed keywords into named lists for content planning, campaigns, or topic clusters.

> **Prerequisite:** Read `../thekeyword-setup/SKILL.md` for auth, API base URL, and plan limits.

## Plan Limits on Lists

| Plan | Max Lists |
|------|-----------|
| Free | 1 |
| Starter | 10 |
| Pro | 50 |
| Ultra | 200 |

## List All Lists

```
GET /api/v1/lists?limit=25
```

Returns paginated list of all keyword lists with their names and keyword counts.

Supports cursor pagination: if `has_more` is `true`, pass `next_cursor` as `cursor`.

## Create a List

```
POST /api/v1/lists
Content-Type: application/json

{
  "name": "Q1 Content Ideas",
  "description": "Keywords to target for Q1 blog content"
}
```

`name` is required. `description` is optional.

Returns the created list with its `id`.

## View a List

```
GET /api/v1/lists/{id}
```

Returns the list metadata and all keywords in the list with their metrics.

## Add Keywords to a List

```
POST /api/v1/lists/{id}/keywords
Content-Type: application/json

{
  "keywords": ["remote work tools", "hybrid office software"],
  "notes": "Found these during competitor analysis"
}
```

`keywords` is an array of keyword text (not slugs, not IDs). The keywords must already exist in the database (i.e., they must have been analyzed first).

`notes` is optional — use it to annotate why these keywords were added.

## Delete a List

```
DELETE /api/v1/lists/{id}
```

Returns HTTP 204 on success. This deletes the list but does not delete the keywords themselves.

## Workflow

### Create and populate a list

1. `GET /api/v1/usage` — Check plan limits
2. `POST /api/v1/lists` — Create the list
3. `GET /api/v1/keywords?sort=-search_volume&limit=20` — Find keywords to add
4. `POST /api/v1/lists/{id}/keywords` — Add selected keywords
5. Confirm to user: list created with N keywords

### View an existing list

1. `GET /api/v1/lists` — Show all lists
2. `GET /api/v1/lists/{id}` — Show keywords in selected list
3. Present as a table with metrics

### Add to an existing list

1. `GET /api/v1/lists` — Show lists for selection
2. Confirm which list to add to
3. `POST /api/v1/lists/{id}/keywords` — Add keywords
4. Confirm: N keywords added

## Examples

**User:** "Create a list called 'Blog Q1' and add my top 5 keywords"

1. `POST /api/v1/lists` with `{"name": "Blog Q1"}`
2. `GET /api/v1/keywords?sort=-search_volume&limit=5`
3. `POST /api/v1/lists/{id}/keywords` with the keyword names
4. Confirm: "Created 'Blog Q1' with 5 keywords"

**User:** "Show me my lists"

1. `GET /api/v1/lists`
2. Present: list name, description, keyword count for each

**User:** "Add 'email marketing' and 'crm tools' to my Content Ideas list"

1. `GET /api/v1/lists` — Find the "Content Ideas" list ID
2. `POST /api/v1/lists/{id}/keywords` with `{"keywords": ["email marketing", "crm tools"]}`
3. Confirm: "Added 2 keywords to Content Ideas"

**User:** "Show what's in my SEO Targets list"

1. `GET /api/v1/lists` — Find "SEO Targets" list ID
2. `GET /api/v1/lists/{id}` — Get full list with keywords
3. Present as table: keyword, volume, difficulty, intent, trend

## Tips

- Keywords must be analyzed before adding to a list — if a keyword hasn't been analyzed, use keyword-analysis first
- Use lists to group keywords by topic, content pillar, campaign, or priority
- The `notes` field on add is useful for tracking why keywords were grouped together
- Deleting a list doesn't delete the keywords — they stay in your library

## Related Skills

- **thekeyword-setup** — API authentication and configuration
- **keyword-analysis** — Analyze keywords before adding to lists
- **keyword-lookup** — Deep-dive into a specific keyword
- **keyword-library** — Browse all keywords to find ones to organize
