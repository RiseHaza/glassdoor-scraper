[Glassdoor Scraper](https://apify.com/cryptosignals/glassdoor-scraper?fpr=data)

# Glassdoor Scraper — Jobs, Reviews & Salary Data

Scrape Glassdoor job listings, company reviews, and salary data at scale — **no login required**. Search by keyword, location, or company name. Extract job titles, salaries, ratings, pros/cons, and more.

## Why Use This Glassdoor Scraper?

Glassdoor is the largest employer review platform, with millions of salary reports and company reviews. Whether you're benchmarking compensation, monitoring employer brand sentiment, or analyzing hiring trends — this scraper gives you structured data without needing a Glassdoor account.

**No login required.** No Glassdoor account needed. The scraper uses Playwright with stealth mode to extract data from publicly accessible pages.

## Features

- **Job listings** — Title, company, location, salary range, rating, posting date
- **Company reviews** — Reviewer title, overall rating, pros, cons, sub-ratings (work-life balance, compensation, career opportunities, culture, management)
- **Both modes** — Scrape jobs and reviews in a single run
- **Salary extraction** — Salary ranges where available
- **Sorting** — Sort by relevance, date, salary (high/low), or rating
- **Pagination** — Automatically follows pages to collect up to 500 results
- **Stealth mode** — Uses Playwright with fingerprint evasion for reliable access

## Input Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `mode` | string | Yes | `jobs` | `jobs`, `reviews`, or `both` |
| `keyword` | string | No | — | Job title or search term (e.g. `"data scientist"`) |
| `location` | string | No | — | City/state/country (e.g. `"New York, NY"`) |
| `companyName` | string | Conditional | — | Company name for reviews (required for `reviews` mode) |
| `maxResults` | integer | No | `5` | Maximum results to return (1–500) |
| `includeSalary` | boolean | No | `true` | Extract salary data when available |
| `sortBy` | string | No | `relevance` | `relevance`, `date`, `salary_high`, `salary_low`, `rating` |

## Example Input

### Search for Jobs

```
{
    "mode": "jobs",
    "keyword": "software engineer",
    "location": "San Francisco, CA",
    "maxResults": 50,
    "sortBy": "date"
}
```

### Get Company Reviews

```
{
    "mode": "reviews",
    "companyName": "Google",
    "maxResults": 100
}
```

### Jobs + Reviews in One Run

```
{
    "mode": "both",
    "keyword": "product manager",
    "companyName": "Meta",
    "location": "New York, NY",
    "maxResults": 30
}
```

## Output Format

### Job Listing

```
{
    "type": "job",
    "title": "Senior Software Engineer",
    "company": "Stripe",
    "location": "San Francisco, CA",
    "salary": "$180K - $250K (Glassdoor est.)",
    "rating": 4.2,
    "easyApply": true,
    "postedDate": "2d ago",
    "url": "https://www.glassdoor.com/job-listing/...",
    "jobType": "Full-time"
}
```

### Company Review

```
{
    "type": "review",
    "company": "Google",
    "reviewerTitle": "Senior Software Engineer",
    "overallRating": 4,
    "pros": "Great compensation, smart colleagues, interesting problems",
    "cons": "Bureaucracy in larger teams, slow promotion cycles",
    "workLifeBalance": 4,
    "compensation": 5,
    "careerOpportunities": 3,
    "culture": 4,
    "management": 3,
    "recommends": true,
    "date": "2026-03-15"
}
```

## How to Use with Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")

# Search for data science jobs in NYC
run = client.actor("cryptosignals/glassdoor-scraper").call(run_input={
    "mode": "jobs",
    "keyword": "data scientist",
    "location": "New York, NY",
    "maxResults": 30,
    "sortBy": "salary_high",
})

for job in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{job['title']} at {job['company']} — {job.get('salary', 'N/A')}")
```

```
# Get company reviews for competitive analysis
run = client.actor("cryptosignals/glassdoor-scraper").call(run_input={
    "mode": "reviews",
    "companyName": "Microsoft",
    "maxResults": 50,
})

for review in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{review['overallRating']}/5 — {review['pros'][:60]}...")
```

## Use Cases

- **Salary benchmarking** — Compare compensation across companies and locations for specific roles
- **Competitive intelligence** — Monitor what competitors' employees say about culture, management, and compensation
- **Job market analysis** — Track hiring trends, in-demand skills, and salary ranges by location
- **Employer branding** — Understand how your company is perceived vs. competitors on Glassdoor
- **HR & recruiting** — Benchmark salaries and identify companies with high turnover signals
- **Due diligence** — Research employee sentiment before acquisitions, partnerships, or job offers
- **Academic research** — Analyze workplace satisfaction trends across industries over time

## Working Around Bot Detection

Glassdoor uses aggressive bot detection including CAPTCHAs, session fingerprinting, and IP-based rate limiting. This scraper uses Playwright with stealth mode and Firefox fingerprinting to bypass basic detection, but for reliable results at scale you'll need residential proxies.

[ThorData residential proxies](https://thordata.partnerstack.com/partner/0a0x4nzh) provide rotating residential IPs that make requests appear to come from real browsers — essential for Glassdoor which aggressively blocks datacenter IPs. Configure them in the actor's proxy settings for consistent, reliable scraping.

**Tips for reliable Glassdoor scraping:**

- Always use residential proxies (datacenter IPs are blocked quickly)
- Keep `maxResults` under 100 for the most reliable results
- Space out runs — avoid hitting the same search multiple times per hour
- Use the `both` mode to get jobs and reviews in a single run instead of two separate runs

## Integrations

- **Google Sheets** — Export salary and review data to spreadsheets
- **Zapier / Make.com** — Get alerts when new jobs match your criteria
- **Slack** — Notify recruiting teams about new job postings
- **API** — Call programmatically from any language using the Apify API

## FAQ

**Is this legal?**
This scraper accesses publicly visible pages on Glassdoor. Always review Glassdoor's Terms of Service and your local laws before scraping.

**Do I need a Glassdoor account?**
No. The scraper accesses publicly visible job listings and review pages without logging in.

**Why do some runs return fewer results than expected?**
Glassdoor's bot detection may block some requests. Using residential proxies significantly improves reliability. Keep `maxResults` reasonable and space out your runs.

**Can I scrape salary data separately?**
Salary data is included in job results when `includeSalary` is enabled (it is by default). There is no separate salary-only mode.