# KeywordSkills

Keyword research skills for Claude Code powered by TheKeyword API.

## Skills

- `/keywordskills:thekeyword-setup` — Configure API access
- `/keywordskills:keyword-analysis` — Analyze keywords
- `/keywordskills:keyword-lookup` — Look up keyword details
- `/keywordskills:keyword-library` — Browse keyword library
- `/keywordskills:keyword-lists` — Manage keyword lists

## Quick Start

1. Get an API key at https://thekeyword.io/dashboard/api-keys
2. For best experience, set up the MCP server in your Claude Code config:

```json
{
  "mcpServers": {
    "thekeyword": {
      "command": "npx",
      "args": ["@thekeyword/mcp"],
      "env": { "THEKEYWORD_API_KEY": "tk_live_..." }
    }
  }
}
```

3. Ask Claude to research keywords — skills activate automatically.
