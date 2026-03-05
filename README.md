# Keyword Research Skills for AI Agents

A collection of AI agent skills for keyword research powered by [TheKeyword](https://thekeyword.io). Analyze keywords, check search volume and difficulty, browse your keyword library, and organize keywords into lists. Works with Claude Code, Cursor, Codex, Gemini CLI, Windsurf, and any agent that supports the [Agent Skills](https://agentskills.io) spec.

Built by [TheKeyword](https://thekeyword.io). Need keyword intelligence for your SEO workflow? [Get started free](https://thekeyword.io).

## What are Skills?

Skills are markdown files that give AI agents specialized knowledge and workflows for specific tasks. When you install these skills, your agent knows how to research keywords, check metrics, and organize your findings using TheKeyword API.

## Available Skills

| Skill | Description |
|-------|-------------|
| **thekeyword-setup** | Foundation skill: API authentication, country/geo configuration, plan tiers and limits. Read by all other skills first. |
| **keyword-analysis** | Analyze keywords to get search volume, difficulty, CPC, intent, and trends. Supports single and batch analysis with job polling. |
| **keyword-lookup** | Look up a specific keyword's full data including related keywords, SERP analysis, and content briefs. |
| **keyword-library** | Browse, filter, and sort all your analyzed keywords. Filter by country, intent, trend direction, difficulty range, and more. |
| **keyword-lists** | Create named lists and organize keywords into groups for content planning, campaigns, or topic clusters. |

## How Skills Work Together

```
                    ┌─────────────────────────┐
                    │    thekeyword-setup      │
                    │  (API auth, geo, plans)  │
                    └────────────┬────────────┘
                                 │
         ┌───────────┬───────────┼───────────┬───────────┐
         ▼           ▼           ▼           ▼           │
   ┌───────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │
   │ keyword-  │ │keyword- │ │keyword- │ │keyword- │   │
   │ analysis  │ │ lookup  │ │ library │ │  lists  │   │
   │           │ │         │ │         │ │         │   │
   │ Analyze   │ │ Check   │ │ Browse  │ │Organize │   │
   │ single or │ │ one     │ │ all     │ │keywords │   │
   │ batch     │ │keyword's│ │keywords,│ │into     │   │
   │ keywords  │ │full data│ │filter,  │ │named    │   │
   │           │ │         │ │sort     │ │lists    │   │
   └─────┬─────┘ └─────────┘ └────┬────┘ └────┬────┘   │
         │                        │            │        │
         └── analysis → lookup ───┘            │        │
              library → lists ─────────────────┘        │
              analysis → library ───────────────────────┘
```

## Installation

### Option 1: Skills CLI (recommended, works with all agents)

```bash
npx skills add andginja/keywordskills
```

This works with Claude Code, Cursor, Codex, Windsurf, Gemini CLI, and 40+ other agents.

### Option 2: Claude Code Plugin

```
/plugin marketplace add andginja/keywordskills
/plugin install keywordskills
```

### Option 3: Clone and Copy

```bash
git clone https://github.com/andginja/keywordskills.git
cp -r keywordskills/skills/* .claude/skills/
```

### Option 4: Git Submodule

```bash
git submodule add https://github.com/andginja/keywordskills.git .claude/keywordskills
```

## Prerequisites

1. A [TheKeyword](https://thekeyword.io) account
2. An API key from [thekeyword.io/dashboard/api-keys](https://thekeyword.io/dashboard/api-keys)

### Enhanced Setup (Claude Code)

For the best experience with Claude Code, set up the MCP server for automatic authentication:

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

## Usage

Once installed, just ask your agent to help with keyword research:

**"Analyze the keyword 'remote work tools' for the US market"**
→ Uses keyword-analysis skill

**"Show me my top keywords by volume"**
→ Uses keyword-library skill

**"What's the difficulty for 'crm software' in Portugal?"**
→ Uses keyword-lookup skill

**"Create a list called Q1 Content and add my best keywords"**
→ Uses keyword-lists skill

## Plan Tiers

Different plans unlock different features:

| Feature | Free | Starter | Pro | Ultra |
|---------|------|---------|-----|-------|
| Monthly credits | 3 | 200 | 1,000 | 5,000 |
| Batch analysis | -- | 10/batch | 50/batch | 100/batch |
| Lists | 1 | 10 | 50 | 200 |
| Signals | -- | Yes | Yes | Yes |
| Forecasts | -- | -- | Yes | Yes |
| Content briefs | -- | -- | Yes | Yes |
| Discovery | -- | -- | Yes | Yes |

## Supported Countries

US, GB, CA, AU, DE, FR, ES, IT, PT, BR, NL, BE, AT, CH, IE, NZ, IN, MX, AR, CO

## Contributing

Found a way to improve a skill or have a new one to add? PRs and issues welcome.

## License

MIT — Use these however you want.
