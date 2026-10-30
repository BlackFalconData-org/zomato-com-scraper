# Zomato Restaurant Scraper

Extract structured data from [zomato.com](https://zomato.com) — Zomato restaurants in Indian cities, by cuisine or city page. Each row carries the rating, cost for two and contact details as Zomato lists them, plus the FSSAI licence number. Incremental mode flags new and changed restaurants between runs.

**[Zomato Restaurant Scraper on Apify →](https://apify.com/blackfalcondata/zomato-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by ↕️ reviews sort order, 📝 description format, and more.

**Detail enrichment** — Fetch contact information, structured metadata for each restaurant.

**Incremental mode** — Only get new or changed restaurants since your last run. Content hash per restaurant — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how restaurants evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N restaurants. Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every restaurant returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured restaurants from zomato.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor restaurants, track trends, and analyze market dynamics with structured, deduplicated data from zomato.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per restaurant is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "biryani",
  "location": "bangalore",
  "maxResults": 100,
  "includeDetails": true,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Cuisine or dish keyword, e.g. "biryani", "pizza", "north indian". Leave empty to browse the city's full listing. Use a JSON array for several terms, e.g. ["biryani","pizza"]. |
| `location` | string | — | Zomato city, e.g. "bangalore", "mumbai", "delhi ncr". Defaults to Bangalore. Use a JSON array for several cities. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste Zomato listing pages (a city delivery page, a city dine-out page such as /mumbai/restaurants, or a city cuisine page such as /mumbai/restaurants/biryani) and individual restaurant pages. Each URL becomes its own task; results are merged and deduped across all URLs. Filters in a pasted URL are not supported and stop the run with an input error: remove them or use the query/city fields. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Visit each restaurant page for address, phone, coordinates, opening hours, FSSAI licence, pure-veg flag and popular dishes. Turn off for a faster, listing-only run. |
| `includeMenu` | boolean | `false` | Add a `menu` array to each restaurant with every dish: name, price, category, veg/non-veg, description and image. Uses the same restaurant page request as Include Full Details, so there is no extra request when details are on. |
| `includeReviews` | boolean | `false` | Also collect each restaurant's customer reviews (text, star rating, reviewer, date, likes). One extra request per 5 reviews per restaurant. |
| `maxReviewsPerRestaurant` | integer | `20` | Reviews per restaurant when Include Reviews is on (0 = all reviews). |
| `reviewsSort` | enum | `"newest"` | Which reviews to collect first when Include Reviews is on. Most liked first reads every review of the restaurant and ranks them by likes (the site cannot sort by likes itself), so it takes longer on restaurants with many reviews. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every restaurant returns the same 47-field schema. Missing values are `null` — never omitted.

- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`
- `restaurantName`
- `city`
- `locality`
- `cuisines`
- `costForTwo`
- `rating`
- `totalReviews`
- `isPureVeg`
- `address`
- `fssaiNumber`
- `popularItems`
- `outletUrl`
- `portalUrl`
- `restaurantId`
- `title`
- `location`
- `restaurantUrl`
- `ratingText`
- `deliveryRating`
- `deliveryReviews`
- `costForOne`
- `priceValue`
- `priceCurrency`
- `imageUrl`
- `isPromoted`
- `country`
- `zipcode`
- `latitude`
- `longitude`
- `fullAddress`
- `phone`
- `openingHours`
- `timingText`
- `isPermanentlyClosed`
- `isTemporarilyClosed`
- `knownFor`
- `peopleSay`
- `popularItemsSource`
- `menuItemCount`

---

## Sample output

One object per restaurant. Here is a real example from a production run:

```json
{
  "listingId": "aa8f618c4e2e8005b4f974a1b5385c5360928147a6387ad75b82f49a8db9a39c",
  "searchQuery": "delivery",
  "contentQuality": "full",
  "detailFetched": true,
  "scrapedAt": "2026-10-26T16:33:56.262Z",
  "source": "Zomato",
  "contentHash": null,
  "changeType": null,
  "restaurantName": "ITC Aashirvaad Soul Creations",
  "city": "Bangalore",
  "locality": "Shivajinagar",
  "cuisines": [
    "North Indian",
    "Vegetarian",
    "Healthy Food",
    "Indian",
    "South Indian",
    "Mithai"
  ]
}
```

*Truncated — full records contain 47 fields. See Output fields for the complete schema.*

**[Try Zomato Restaurant Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/zomato-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002691 |

See the [actor on Apify](https://apify.com/blackfalcondata/zomato-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape zomato.com?**
Use this actor on Apify to extract structured data from zomato.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get zomato.com data as JSON, CSV, or Excel?**
The actor writes each restaurant to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape zomato.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check zomato.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each restaurant gets a content hash. On subsequent runs, only new or changed restaurants are emitted — saving time, compute, and storage. Expired restaurants can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Zomato Restaurant Scraper](https://apify.com/blackfalcondata/zomato-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
