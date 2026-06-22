[Glassdoor Scraper](https://apify.com/alizarin_refrigerator-owner/glassdoor-scraper?fpr=data)

Scrape Glassdoor for company reviews, salaries, interview experiences, and CEO approval ratings. Look up any company by domain/website URL — no need to find the Glassdoor page yourself. For HR research, employer branding, salary benchmarking, and competitive intelligence.

**BYOK (Bring Your Own Key)** -- you provide your own API credentials.

---

## Before You Start

This actor requires your own API credentials to fetch real data.

**Where to get your key:** JSON array of cookies from Cookie-Editor. Helps bypass Glassdoor login walls and paywalls.

You can test with **Demo Mode** first (free, no key needed) to see the output format before committing.

---

## Quick Start

### Test with Demo Mode (free, no API key needed)

```
{
  "demoMode": true,
  "companyUrl": "https://example.com",
  "companyDomain": [
    "example.com"
  ]
}
```

### Run with real data

```
{
  "demoMode": false,
  "scrapeType": "company_profile",
  "companyUrl": "https://example.com",
  "companyDomain": [
    "competitor1.com",
    "competitor2.com"
  ],
  "companySize": "any",
  "includeReviews": true,
  "maxReviewsPerCompany": 25,
  "includeSalaries": true,
  "includeInterviews": false,
  "reviewFilter": "all",
  "sortBy": "date",
  "maxResults": 50,
  "sessionCookies": "YOUR_API_KEY_HERE",
  "webhookPlatform": "custom",
  "cookieKvStoreName": "cookie-sessions"
}
```

---

## Input Parameters

| Parameter | Type | Default | Required | Description |
| --- | --- | --- | --- | --- |
| `scrapeType` | string | `"company_profile"` | No | Type of data to scrape |
| `companyUrl` | string | - | No | Direct Glassdoor company page URL (e.g., [https://www.glassdoor.com/Overview/Working-at-Google-EI_IE9079.htm](https://www.glassdoor.com/Overview/Working-at-Google-EI_IE9079.htm)) |
| `companyDomain` | string | - | No | Company website or domain to look up on Glassdoor (e.g., "stripe.com" or "[https://www.hubspot.com](https://www.hubspot.com)"). The actor resolves this to the correct Glassdoor page automatically. |
| `companyName` | string | - | No | Company name to search for |
| `location` | string | - | No | Filter by location (city, state, or country) |
| `industry` | string | - | No | Filter by industry (Technology, Healthcare, Finance, etc.) |
| `companySize` | string | `"any"` | No | Filter by company size |
| `minRating` | number | - | No | Minimum overall rating (1.0-5.0) |
| `includeReviews` | boolean | `true` | No | Scrape employee reviews |
| `maxReviewsPerCompany` | integer | `25` | No | Maximum reviews to collect per company |
| `includeSalaries` | boolean | `true` | No | Scrape salary information |
| `includeInterviews` | boolean | `false` | No | Scrape interview experiences |
| `reviewFilter` | string | `"all"` | No | Filter reviews by type |
| `sortBy` | string | `"date"` | No | How to sort reviews |
| `maxResults` | integer | `50` | No | Maximum number of companies to scrape |
| `sessionCookies` | string | - | Yes* | JSON array of cookies from Cookie-Editor. Helps bypass Glassdoor login walls and paywalls. |
| `proxyConfiguration` | object | - | No | Proxy settings for the scraper |
| `demoMode` | boolean | `true` | No | Return sample data without actual scraping (for testing) |
| `webhookUrl` | string | - | No | URL to POST results when scraping completes (Zapier, Make, n8n, custom endpoint) |
| `webhookPlatform` | string | `"custom"` | No | Platform-specific payload formatting |
| `webhookHeaders` | object | - | No | Custom HTTP headers to send with webhook (JSON object) |
| `cookieStorageKey` | string | - | No | Key name to load cookies from the Cookie Manager KV store. If set and no manual sessionCookies are provided, the actor loads cookies from the named KV store automatically. Use this with the Cookie Manager actor for automated cookie rotation. |
| `cookieKvStoreName` | string | `"cookie-sessions"` | No | Name of the Apify Key-Value store where Cookie Manager saves cookies. Defaults to 'cookie-sessions' if not set. |

*Required when Demo Mode is off.

---

## Pricing

This actor uses **pay-per-event** billing:

| Event | Description | Price |
| --- | --- | --- |
| Actor Start | Base cost per run | $0.10 |
| Company Scraped | Each Glassdoor company profile scraped | $0.08 |
| Review Scraped | Each employee review extracted | $0.01 |
| Salary Scraped | Each salary entry extracted | $0.01 |
| Dataset Item | Each result stored in dataset | $0.00 |

**Demo mode is free** -- no charges for sample data.

---

## Troubleshooting

### "API key is required"

You have Demo Mode turned **off** but didn't provide an API key. Either:

- Turn Demo Mode **on** to test with sample data
- Add your API key in the input

### "API error 403" or "Unauthorized"

Your API key is invalid, expired, or doesn't have access to this specific API endpoint. Double-check your key and account permissions.

### "API error 429" or "Rate limit"

Too many requests. Wait a minute and try again, or reduce the number of items per run.

### No results or empty dataset

Check the run log for error messages. Common causes:

- Invalid input format (check the examples above)
- API key without proper permissions
- The target data doesn't exist or is too small to track

### How do I test without an API key?

Enable **Demo Mode** in the input. This returns realistic sample data so you can verify the output format works for your workflow.

---

**Built by John Rippy | [Actor Arsenal](https://actorarsenal.com)**