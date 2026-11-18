---
name: competitor-review-complaints
description: Find out what users hate about any app. Collects recent 1-2 star reviews (App Store for free via Apple's public feed, or both App Store and Google Play via an optional scraper, or reviews the user pastes or uploads), then groups the complaints into ranked themes with real quotes, versions and platform split. Use when asked to analyse app reviews, competitor weaknesses, churn reasons, feature gaps, or "what do people complain about in <app>".
---

# Competitor review complaints

Turn an app's low-star reviews into a ranked list of complaint themes you can act on.

## Step 1: Get the reviews (pick one source)

### A. Free: Apple App Store public feed (no account needed)
1. Find the app's numeric App Store ID. It's the number after `id` in its App Store URL, e.g. `apps.apple.com/us/app/duolingo/id570060128` gives `570060128`. If you only have a name, look it up with `https://itunes.apple.com/search?term=<name>&entity=software&limit=3`.
2. Fetch up to 10 pages (50 reviews each) of the public feed:
   `https://itunes.apple.com/<country>/rss/customerreviews/page=<1..10>/id=<APP_ID>/sortby=mostrecent/json`
3. From `feed.entry`, use entries that have `im:rating`. Keep only ratings 1–2. Useful fields: `im:rating.label`, `title.label`, `content.label`, `im:version.label`, `updated.label`.
4. Apple caps this at about 500 recent reviews per country. Repeat with other country codes (`us`, `gb`, `ca`, `au` …) for more.

### B. Free: reviews the user provides
Accept pasted text or a CSV/JSON export, for example from App Store Connect or Google Play Console for the user's own app. Ask which columns hold the rating, text, date and version.

### C. Optional paid scraper: both stores and many countries in one run
If the Apify MCP server is connected (`https://mcp.apify.com?tools=fetchwelldata/app-store-play-reviews`), run `call-actor` on `fetchwelldata/app-store-play-reviews`:
```json
{ "apps": ["<App Store / Google Play links or IDs>"], "maxRating": 2, "maxReviewsPerApp": 300, "countries": ["us"] }
```
Then use `get-dataset-items` with the run's `datasetId`. Rows with `"type": "review"` are reviews. This adds Google Play and multi-country in one step. It's pay-per-use at about $0.50 per 1,000 reviews, and Apify's free monthly credit covers small runs. Disclosure: this scraper is made by the skill's author.

## Step 2: Analyse
- Read the review texts and cluster complaints into 5–8 themes (e.g. paywall/limits, bugs after update, billing, missing feature X, ads).
- For each theme give:
  - the share of low-star reviews (%), noting that one review can fall under several themes
  - the platform split, if both stores are present
  - versions linked to spikes
  - two short real quotes (≤ 20 words each) from different reviews
- Note whether the developer replies, if reply data exists.

## Step 3: Report
- **One-line verdict per app:** the #1 complaint and how dominant it is.
- **Ranked theme table:** theme · share · platform split · example quotes.
- **Opportunities:** 3 concrete things a competitor could do better, each tied to a theme.
- **Method note:** sources, review count, date range, and that shares are approximate.

## Rules
- Only quote text that's actually in the data. Never invent reviews or numbers.
- Leave out reviewer names.
- Say how many reviews were analysed, and from which stores and dates.
