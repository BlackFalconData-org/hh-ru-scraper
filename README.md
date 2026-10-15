# hh.ru Job Scraper - Salary, Employer, Remote

Extract structured data from [hh.ru](https://hh.ru) — structured job listings from hh.ru: title, employer, salary range and currency, area, experience, schedule and remote flags. Track new and changed vacancies with incremental runs, or export slim records for AI agents.

**[hh.ru Job Scraper - Salary, Employer, Remote on Apify →](https://apify.com/blackfalcondata/hh-ru-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, salary data, contact information for each job.

**Incremental mode** — Only get new or changed jobs since your last run. Content hash per job — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how jobs evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N jobs (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every job returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured jobs from hh.ru on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor jobs, track trends, and analyze market dynamics with structured, deduplicated data from hh.ru.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on hh.ru to inform pricing decisions, hiring plans, or candidate negotiations.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per job is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "python",
  "location": "113",
  "maxResults": 50
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords, exactly as you would type them on hh.ru (Russian or English). Leave empty to browse everything in the selected area. Use a JSON array for several searches in one run, e.g. ["python","аналитик данных"]. |
| `location` | string | — | hh.ru area id — 113 = Russia (whole country), 1 = Moscow, 2 = Saint Petersburg, 40 = Kazakhstan, 16 = Belarus. Leave empty for no area filter. Use a JSON array for several areas, e.g. ["1","2"]. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by job ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows) — drops the verbose raw vacancy objects and keeps the flat job fields. |
| `excludeEmptyFields` | boolean | `false` | Drop null / empty-string / empty-array fields (and fully-null nested objects) from each output record. Off by default to preserve dataset schema compatibility; turn on to slim output for LLM / agent consumption. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a key derived from the search inputs (search terms, areas, start URLs) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | Also emit jobs that have not changed since the last run (incrementalMode only). |
| `emitExpired` | boolean | `false` | Also emit jobs that have disappeared since the last run (incrementalMode only). |
| `skipReposts` | boolean | `false` | When incremental, skip jobs whose content matches an expired job from a prior run (cross-run repost detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED jobs. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of jobs included in each notification message (1–20). |
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
| `includeDetails` | boolean | `false` | Fetch the vacancy page for each listing to add the full description (text, HTML and Markdown), the key skills and any phone numbers found in the text. Increases run time (one extra request per vacancy). Leave off for a fast, low-cost run. |
| `enrichEmails` | boolean | `false` | Find a public contact email for each employer by reading its own website (homepage plus its contact and about pages), into contactEmail, contactEmails and contactWebsite. Also pulls addresses written in the full vacancy description into email and emails when Include Full Listing Details is on. |
| `lat` | number | — | Latitude of the centre point. Pair with lon and Search Radius: vacancies whose address pin lies outside the radius are dropped, and every kept vacancy gets distanceKm. Vacancies without an address pin are kept with distanceKm null. |
| `lon` | number | — | Longitude of the centre point. Pair with lat and Search Radius. Both coordinates are needed for the filter to apply. |
| `radiusKm` | integer | — | Keep only vacancies pinned within this many kilometres of lat/lon (great-circle distance on the address pin). 0 or empty only adds distanceKm and filters nothing. Vacancies without a pin are never dropped. |

---

## Output fields

Every job returns the same 112-field schema. Missing values are `null` — never omitted.

- `@workSchedule`
- `@showContact`
- `@responseLetterRequired`
- `vacancyId`
- `name`
- `company`
- `compensation`
- `publicationTime`
- `area`
- `acceptTemporary`
- `address`
- `contactInfo`
- `creationSite`
- `creationSiteId`
- `displayHost`
- `lastChangeTime`
- `creationTime`
- `canBeShared`
- `employerManager`
- `inboxPossibility`
- `chatWritePossibility`
- `notify`
- `links`
- `acceptIncompleteResumes`
- `driverLicenseTypes`
- `languages`
- `workingDays`
- `workingTimeIntervals`
- `workingTimeModes`
- `vacancyProperties`
- `vacancyPlatforms`
- `professionalRoleIds`
- `workExperience`
- `employment`
- `closedForApplicants`
- `userTestPresent`
- `employmentForm`
- `flyInFlyOutDurations`
- `sideJob`
- `internship`
- `nightShifts`
- `workFormats`
- `workScheduleByDays`
- `workingHours`
- `experimentalModes`
- `acceptLaborContract`
- `civilLawContracts`
- `autoResponse`
- `inclusivenessTypes`
- `@isAdv`
- `clickUrl`
- `viewUrl`
- `userLabels`
- `snippet`
- `responsesCount`
- `totalResponsesCount`
- `online_users_count`
- `tags`
- `labels`
- `extraLabels`
- `searchRid`
- `positionInRequest`
- `id`
- `title`
- `location`
- `url`
- `portalUrl`
- `company_name`
- `company_id`
- `company_url`
- `area_id`
- `salary_from`
- `salary_to`
- `salary_currency`
- `salary_gross`
- `salary`
- `distanceKm`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `keySkills`
- `phones`
- `phone`
- `emails`
- `email`
- `date_posted`
- `date_created`
- `isPromoted`
- `published_at`
- `requirement_snippet`
- `responsibility_snippet`
- `experience`
- `employment_type`
- `schedule`
- `is_remote`
- `rank_on_page`
- `page`
- `keyword`
- `search_area`
- `scraped_at`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`

---

## Sample output

One object per job. Here is a real example from a production run:

```json
{
  "@workSchedule": "fullDay",
  "@showContact": true,
  "@responseLetterRequired": false,
  "vacancyId": 138337591,
  "name": "Fullstack-разработчик",
  "company": {
    "@showSimilarVacancies": true,
    "@trusted": true,
    "@category": "COMPANY",
    "@countryId": 1,
    "@employerWithSpecificCountryLaws": false,
    "@state": "APPROVED",
    "id": 13026325,
    "name": "Магницкая Анна Валерьевна",
    "visibleName": "Магницкая Анна Валерьевна",
    "employerOrganizationFormId": 9,
    "companySiteUrl": "",
    "accreditedITEmployer": false,
    "employerOnAdditionalCheck": false
  },
  "compensation": {
    "from": 100000,
    "to": 250000,
    "currencyCode": "RUR",
    "gross": false,
    "perModeFrom": 100000,
    "perModeTo": 250000,
    "mode": "MONTH",
    "frequency": "TWICE_PER_MONTH"
  },
  "publicationTime": {
    "@timestamp": 1791728257,
    "$": "2026-10-11T17:17:37.459+03:00"
  },
  "area": "Иваново (Ивановская область)",
  "acceptTemporary": false,
  "address": {
    "@id": 29951983,
    "@disabled": false,
    "@wrong": false,
    "city": "Иваново",
    "street": "улица Зверева",
    "building": "8",
    "displayName": "Иваново, улица Зверева, 8",
    "mapData": "{\"points\":{\"center\":{\"lat\":57.001235,\"lng\":40.968998,\"zoom\":14},\"marker\":{\"lat\":57.001235,\"lng\":40.968998}}}",
    "marker": {
      "@lat": 57.001235,
      "@lng": 40.968998
    },
    "manager": {
      "@id": 16873198
    },
    "districtDto": {
      "id": 293,
      "isMicroDistrict": false,
      "name": "Фрунзенский"
    }
  },
  "contactInfo": null
}
```

*Truncated — full records contain 112 fields. See Output fields for the complete schema.*

**[Try hh.ru Job Scraper - Salary, Employer, Remote now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/hh-ru-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00009 |
| Result | $0.0018 |

See the [actor on Apify](https://apify.com/blackfalcondata/hh-ru-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape hh.ru?**
Use this actor on Apify to extract structured data from hh.ru. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get hh.ru data as JSON, CSV, or Excel?**
The actor writes each job to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape hh.ru?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check hh.ru's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each job gets a content hash. On subsequent runs, only new or changed jobs are emitted — saving time, compute, and storage. Expired jobs can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [hh.ru Job Scraper - Salary, Employer, Remote](https://apify.com/blackfalcondata/hh-ru-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
