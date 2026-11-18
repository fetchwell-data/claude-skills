# Fetchwell Data: free Claude skills and n8n templates

Ready-to-use AI and automation workflows for market research, competitor analysis and news monitoring. Each workflow runs on a reliable, pay-per-use data tool on the [Apify Store](https://apify.com/fetchwelldata).

## Claude skills
| Skill | What it does | Data tool |
|---|---|---|
| [competitor-review-complaints](skills/competitor-review-complaints/SKILL.md) | Pulls 1–2★ reviews from the App Store and Google Play and ranks complaint themes with real quotes, versions and iOS/Android split | [App Store & Google Play Reviews](https://apify.com/fetchwelldata/app-store-play-reviews) |
| [news-and-trends-briefing](skills/news-and-trends-briefing/SKILL.md) | A one-page briefing on any topic: what the press publishes plus what people search, rising queries and content gaps | [Google News](https://apify.com/fetchwelldata/google-news-reliable) + [Google Trends](https://apify.com/fetchwelldata/google-trends-reliable) |

### Install a skill
**Claude Code:** copy the skill folder into `~/.claude/skills/` (or your project's `.claude/skills/`).

**Claude.ai / Claude Desktop:** upload the skill folder as a custom skill under Settings → Capabilities.

### Connect the data (Apify MCP server)
```bash
# Claude Code
claude mcp add --transport http apify "https://mcp.apify.com?tools=fetchwelldata/app-store-play-reviews,fetchwelldata/google-news-reliable,fetchwelldata/google-trends-reliable"
```
In Claude Desktop, add `https://mcp.apify.com?tools=...` (same list) as a custom connector. You'll sign in to Apify on first use.

## n8n templates
| Template | What it does |
|---|---|
| [google-news-to-sheets.json](n8n/google-news-to-sheets.json) | Every morning, appends the last 24 h of Google News for your keywords (headline, source, date, real publisher URL) to a Google Sheet |

**Setup:** import the JSON into n8n, then:
1. Add a **Header Auth** credential to the HTTP node with Name `Authorization` and Value `Bearer <your Apify API token>`.
2. Connect Google Sheets and paste your sheet URL.
3. Edit the keywords.

## Costs
The workflows are free. The data tools are pay-per-use on Apify, typically cents per run:
- Reviews: about $0.50 per 1,000
- News: $2 per 1,000 articles
- Trends: $3.50 per 1,000 results

Failed items are never charged.

## About
Built by **Fetchwell Data**: reliable data tools for analysts, marketers and AI agents. Issues and ideas are welcome. Open an issue here or on the Actor's Apify page.

*Not affiliated with Apple, Google, Anthropic or n8n.*

License: MIT
