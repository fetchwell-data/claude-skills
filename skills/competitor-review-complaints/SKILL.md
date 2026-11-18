---
name: competitor-review-complaints
description: Find out what users hate about any app. Pulls recent 1-2 star reviews from the Apple App Store and Google Play (one or several competitor apps), then groups the complaints into ranked themes with real quotes, versions and countries. Use when asked to analyse app reviews, competitor weaknesses, churn reasons, feature gaps, or "what do people complain about in <app>".
---

# Competitor review complaints

Turn an app's low-star reviews from **both** app stores into a ranked list of complaint themes you can act on.

## Requirements
- The **Apify MCP server** connected to Claude, with this Actor available:
  `https://mcp.apify.com?tools=fetchwelldata/app-store-play-reviews`
  (Claude Code: `claude mcp add --transport http apify "https://mcp.apify.com?tools=fetchwelldata/app-store-play-reviews"`. Claude Desktop: add it as a custom connector.)
- An Apify account. The Actor costs about $0.50 per 1,000 reviews, so a typical analysis of 600 reviews costs about $0.30.

## Steps
1. **Identify the apps.** Ask for app names or store links if the user hasn't given them. App Store links, Google Play links, Apple numeric IDs and Play package names all work. If only a name is given, use `searchTerms` instead of `apps`.
2. **Run the Actor** with `call-actor` on `fetchwelldata/app-store-play-reviews`:
   ```json
   {
     "apps": ["<links or ids>"],
     "searchTerms": ["<app name, only if no links>"],
     "searchResultsPerStore": 1,
     "maxRating": 2,
     "maxReviewsPerApp": 300,
     "sortBy": "newest",
     "countries": ["us"],
     "includeAppDetails": true
   }
   ```
   Add more countries if the user cares about specific markets (e.g. `["us","gb","ae"]`).
3. **Fetch results** with `get-dataset-items` using the run's `datasetId`. Rows with `"type": "app"` are app details; rows with `"type": "review"` are reviews.
4. **Analyse.** Read the review texts and:
   - Cluster complaints into 5–8 themes (e.g. crashes after update, billing/subscription, missing feature X, ads, login problems).
   - For each theme give: share of reviews (%), trend (more common in recent versions?), store split (iOS vs Android), and 2 short real quotes (≤ 20 words each, from different reviews).
   - Note versions that triggered spikes (`appVersion`) and whether the developer replies (`replyText`, Google Play).
5. **Report** in this shape:
   - One-line verdict per app: rating, rating count, the #1 complaint.
   - Ranked theme table: theme · share · iOS/Android · example quotes.
   - "Opportunities": 3 concrete things a competitor could do better, each tied to a theme.

## Notes
- Apple's public feed gives up to 500 recent reviews per app per country, so add countries for more iOS coverage.
- Reviewer names are excluded by default. Keep it that way unless there's a lawful reason to include them.
- Don't invent quotes. Only quote review text that is actually in the dataset.
