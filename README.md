[Glassdoor Scraper](https://apify.com/sovereigntaylor/glassdoor-scraper?fpr=data)

Extract employee reviews, salary data, company ratings, and employer profiles from Glassdoor. Built for HR analytics, competitive intelligence, and salary benchmarking.

## What It Scrapes

### Company Profile

- Company name, overall rating, review count
- Recommend-to-friend rate, CEO approval rating
- Industry, headquarters, company size, revenue, year founded
- Detailed rating breakdown (culture, work-life balance, compensation, career opportunities, management, diversity)

### Employee Reviews

- Review title, pros, cons, advice to management
- Overall rating and sub-category ratings
- Job title, employment status (current/former), length of employment
- Review date, location, helpful count
- Featured review flag

### Salary Data

- Job title and compensation breakdown
- Base pay (min/median/max)
- Total pay (min/median/max)
- Additional pay (bonuses, stock, commission)
- Sample count (number of salary reports), pay period, currency

## Use Cases

### HR Analytics & Workforce Planning

- Monitor employee sentiment trends over time
- Identify recurring themes in pros/cons across departments
- Benchmark your company's ratings against competitors
- Track CEO approval and recommendation rates

### Competitive Intelligence

- Compare employer brands across your industry
- Analyze competitor strengths and weaknesses from employee perspective
- Monitor competitor culture, management quality, and work-life balance scores
- Identify talent retention risks at competitor companies

### Salary Benchmarking & Compensation Analysis

- Build compensation benchmarks by job title and company
- Compare base pay, total pay, and additional compensation
- Identify pay gaps across roles and companies
- Support salary negotiation with market data

### Recruitment & Talent Acquisition

- Evaluate employer brand before approaching candidates
- Understand candidate expectations for specific companies
- Identify companies with high turnover (many "former employee" reviews)
- Research company culture fit for recruitment pitches

## Input

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `companyUrls` | array | `[]` | Glassdoor company URLs (overview, reviews, or salaries pages) |
| `searchTerm` | string | `""` | Search for a company by name |
| `maxReviews` | integer | `25` | Max reviews per company (0 = unlimited) |
| `includeSalaries` | boolean | `true` | Also scrape salary data |
| `proxyConfiguration` | object | — | Proxy settings (residential strongly recommended) |

### Input Example

```
{
    "companyUrls": [
        "https://www.glassdoor.com/Overview/Working-at-Google-EI_IE9079.htm",
        "https://www.glassdoor.com/Reviews/Microsoft-Reviews-E1651.htm"
    ],
    "maxReviews": 50,
    "includeSalaries": true
}
```

### Search by Name

```
{
    "searchTerm": "Stripe",
    "maxReviews": 25,
    "includeSalaries": true
}
```

## Output

Data is exported to the default dataset in JSON format. Each record has a `type` field: `company`, `review`, or `salary`.

### Company Record

```
{
    "type": "company",
    "companyName": "Google",
    "employerId": "9079",
    "overallRating": 4.3,
    "reviewCount": 45821,
    "recommendRate": "82%",
    "ceoName": "Sundar Pichai",
    "ceoApproval": "91%",
    "industry": "Internet & Web Services",
    "headquarters": "Mountain View, CA",
    "size": "10000+ Employees",
    "revenue": "$10+ billion (USD)",
    "founded": "1998",
    "ratingBreakdown": {
        "cultureAndValues": 4.2,
        "workLifeBalance": 4.0,
        "compensationAndBenefits": 4.5,
        "careerOpportunities": 4.1,
        "seniorManagement": 3.8,
        "diversityAndInclusion": 4.3
    }
}
```

### Review Record

```
{
    "type": "review",
    "reviewTitle": "Great benefits, fast-paced environment",
    "pros": "Excellent compensation, free food, smart colleagues, strong engineering culture",
    "cons": "Work-life balance can be challenging, lots of bureaucracy in larger teams",
    "rating": 4,
    "date": "2026-01-15",
    "jobTitle": "Software Engineer",
    "employmentStatus": "Current Employee",
    "isCurrentJob": true,
    "location": "Mountain View, CA",
    "advice": "Focus more on employee well-being",
    "ratingBreakdown": {
        "workLifeBalance": 3,
        "cultureValues": 4,
        "compensationBenefits": 5,
        "careerOpportunities": 4,
        "seniorManagement": 3,
        "diversityInclusion": 4
    },
    "helpful": 12,
    "companyName": "Google",
    "employerId": "9079"
}
```

### Salary Record

```
{
    "type": "salary",
    "jobTitle": "Software Engineer",
    "basePay": "$165,000/yr",
    "basePayMin": 130000,
    "basePayMax": 210000,
    "basePayMedian": 165000,
    "totalPay": "$245,000/yr",
    "totalPayMin": 180000,
    "totalPayMax": 350000,
    "totalPayMedian": 245000,
    "additionalPay": "$80,000/yr",
    "additionalPayMedian": 80000,
    "sampleCount": 4521,
    "payPeriod": "ANNUAL",
    "currency": "USD",
    "companyName": "Google",
    "employerId": "9079"
}
```

## How It Works

1. **Company Resolution** — Accepts direct Glassdoor URLs or searches by company name
2. **Overview Extraction** — Scrapes the company profile page for employer data
3. **Review Crawling** — Paginates through employee reviews (10 per page)
4. **Salary Crawling** — Paginates through salary data (20 per page, up to 5 pages)
5. **Multi-Strategy Parsing** — Tries Apollo cache (`__APOLLO_STATE__`), then `__NEXT_DATA__`, then DOM selectors

## Proxy Recommendations

Glassdoor has aggressive bot detection. **Residential proxies are strongly recommended** for reliable results.

| Proxy Type | Success Rate | Recommendation |
| --- | --- | --- |
| None | ~10% | Not recommended |
| Datacenter | ~30% | Poor results |
| Residential | ~85% | Recommended |
| Residential (US) | ~90% | Best results |

## Pricing (Pay Per Event)

- **Actor start**: 1 event per run
- **Company profile**: 1 event per company
- **Review scraped**: 1 event per review
- **Salary record**: 1 event per salary entry

## Limitations

- Glassdoor may require authentication for some pages — use proxies
- Review content may be truncated on pages requiring login
- Salary data availability varies by company size and region
- Maximum ~5000 reviews per company per run (to avoid excessive load)
- Rate limited to 12 requests/minute to respect Glassdoor's infrastructure

## Legal Notice

This actor is intended for personal research, HR analytics, and competitive intelligence purposes. Always review and comply with Glassdoor's Terms of Service before scraping. The actor respects rate limits and does not bypass authentication walls.

## Integration — Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")
run = client.actor("sovereigntaylor/glassdoor-scraper").call(run_input={
    "company": "Google",
    "maxResults": 50
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{item.get('title', '')}: {item.get('rating', 'N/A')} stars")
```

## Integration — JavaScript

```
import { ApifyClient } from 'apify-client';
const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });

const run = await client.actor('sovereigntaylor/glassdoor-scraper').call({
    company: 'Google',
    maxResults: 50
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach(item => console.log(item.title || item.name || 'N/A'));
```

## Related Actors

- [Glassdoor Reviews Scraper](https://apify.com/sovereigntaylor/glassdoor-reviews-scraper) — Company reviews in detail
- [Indeed Scraper](https://apify.com/sovereigntaylor/indeed-scraper) — Job listings from Indeed
- [LinkedIn Jobs Scraper](https://apify.com/sovereigntaylor/linkedin-jobs-scraper) — LinkedIn job data
- [Indeed Salary Scraper](https://apify.com/sovereigntaylor/indeed-salary-scraper) — Salary benchmarks
- [Wellfound Scraper](https://apify.com/sovereigntaylor/wellfound-scraper) — Startup job listings