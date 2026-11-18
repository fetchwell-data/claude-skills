---
name: news-and-trends-briefing
description: Daily or weekly briefing on any topic, brand or market that combines what the press is publishing (Google News) with what people are searching (Google Trends) - rising searches, interest over time, and the stories behind spikes. Use when asked for a news briefing, market or brand monitoring, "what's happening with <topic>", trend spotting, or content ideas based on news plus search demand.
---

# News + Trends briefing

One briefing that answers two questions about any topic: **what is being published** and **what people are searching**. The overlap is where the story is.

## Requirements
- The **Apify MCP server** connected to Claude with these Actors:
  `https://mcp.apify.com?tools=fetchwelldata/google-news-reliable,fetchwelldata/google-trends-reliable`
- An Apify account. A typical briefing costs about $0.10–0.30. Google News is $2 per 1,000 articles, Google Trends $3.50 per 1,000 results.

## Steps
1. **Clarify scope.** Get topics or brands (1–5), the country (default US) and the period (default past 7 days). For a daily briefing, use 1 day of news.
2. **News.** Run `fetchwelldata/google-news-reliable` with `call-actor`:
   ```json
   {
     "queries": ["<topic 1>", "<topic 2>"],
     "timeRange": "7d",
     "country": "US",
     "language": "en",
     "maxArticlesPerQuery": 100,
     "resolveArticleUrls": true
   }
   ```
   For busy topics over 30 days, add `"deepMode": true` to get more than 100 articles.
3. **Search demand.** Run `fetchwelldata/google-trends-reliable`:
   ```json
   {
     "searchTerms": ["<topic 1>", "<topic 2>"],
     "dataTypes": ["interestOverTime", "relatedQueries"],
     "geo": "US",
     "timeRange": "today 3-m"
   }
   ```
   To compare brands on one scale, add `"compareTogether": true` (up to 5 terms).
4. **Fetch both datasets** with `get-dataset-items`.
5. **Analyse:**
   - **News:** group articles into 3–6 storylines, count outlets per storyline, and note the most-covered story and any story covered by only one outlet (possible scoop).
   - **Search:** current interest vs the period average, peak dates, and the top 5 **rising** related queries (a "Breakout" value means growth over 5,000%).
   - **Connect them:** did a search spike follow a news story? Is a rising query not yet covered by the press? That's a content opportunity.
6. **Briefing format** (keep it under one page):
   - **Headline:** one sentence on what matters most today.
   - **Top storylines:** title · outlets · link (`articleUrl`).
   - **Search pulse:** trend direction plus the rising queries.
   - **Gaps and opportunities:** 3 bullets.
   - **Watch next:** 2 bullets.

## Notes
- Google Trends values are relative (0–100), not search volumes. Say so if you quote numbers.
- Link to articles. Don't reproduce article text.
- For a recurring briefing, schedule the two Actors in Apify, or run this skill on a schedule.
