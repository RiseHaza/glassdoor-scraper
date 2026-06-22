[Glassdoor Scraper](https://apify.com/swerve/glassdoor-scraper?fpr=data)

# Glassdoor Job Scraper

Scrape job listings from [Glassdoor](https://www.glassdoor.com) with salary estimates, company ratings, full job descriptions, and more. Supports 21 countries with automatic domain health checks and fallback.

## Why This Scraper?

- **Salary data that Glassdoor hides** -- estimated salary range (min, median, max) for each listing, extracted from Glassdoor's internal data
- **Full job descriptions** -- optionally fetch the complete description for every listing (enable `fetchDescriptions`)
- **21 countries** -- dedicated Glassdoor domains for US, UK, Canada, Germany, Australia, and more
- **Smart fallback** -- if a country domain is temporarily down, the scraper automatically falls back to glassdoor.com so your run still produces results
- **In-browser location resolution** -- your location filter is resolved to Glassdoor's internal location ID for accurate results
- **Rich filtering** -- filter by job type, date posted, remote-only, and minimum salary

## Use Cases

- **Recruiters and staffing agencies** pulling fresh job postings across 21 countries to source leads, identify hiring companies, and benchmark against competitors
- **HR and compensation analysts** using Glassdoor's hidden salary estimates to build pay bands and benchmark offers for specific roles and cities
- **Job seekers and career coaches** monitoring new postings at target companies with custom filters (remote, fulltime, posted this week)
- **Sales teams at HR tech vendors** generating leads by tracking which companies are hiring in high volume (signals growth/budget)
- **Market researchers** studying labour demand trends, remote work share, and industry hiring shifts across US, UK, Germany, and more
- **Competitive intelligence teams** tracking how competitors describe roles, benefits, and culture in live listings

## Proxy Required

You **must** use Apify Proxy with the **RESIDENTIAL** group. Glassdoor will block requests from datacenter IPs.

```
{
  "proxyConfig": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `searchQuery` | string | *(required)* | Job search keywords (e.g. `"Product Manager"`) |
| `location` | string |  | City or region to filter by (e.g. `"New York"`) |
| `country` | string | `"us"` | Country code -- see supported list below |
| `maxItems` | integer | `100` | Maximum number of listings to return (max 1000) |
| `jobType` | string | `"all"` | One of: `all`, `fulltime`, `parttime`, `contract`, `internship`, `temporary` |
| `datePosted` | integer |  | Only jobs posted within this many days: `1`, `3`, `7`, `14`, or `30` |
| `remoteOnly` | boolean | `false` | Only return remote positions |
| `minSalary` | number |  | Minimum salary filter |
| `fetchDescriptions` | boolean | `false` | Fetch full job descriptions (slower but much richer data) |
| `proxyConfig` | object |  | Proxy settings -- residential proxy required |

## Supported Countries

| Code | Country | Domain |
| --- | --- | --- |
| `us` | United States | glassdoor.com |
| `uk` | United Kingdom | glassdoor.co.uk |
| `canada` | Canada | glassdoor.ca |
| `india` | India | glassdoor.co.in |
| `australia` | Australia | glassdoor.com.au |
| `germany` | Germany | glassdoor.de |
| `france` | France | glassdoor.fr |
| `netherlands` | Netherlands | glassdoor.nl |
| `austria` | Austria | glassdoor.at |
| `mexico` | Mexico | glassdoor.com.mx |
| `brazil` | Brazil | glassdoor.com.br |
| `belgium` | Belgium | glassdoor.be |
| `switzerland` | Switzerland | glassdoor.ch |
| `ireland` | Ireland | glassdoor.ie |
| `singapore` | Singapore | glassdoor.sg |
| `hong-kong` | Hong Kong | glassdoor.com.hk |
| `new-zealand` | New Zealand | glassdoor.co.nz |
| `israel` | Israel | glassdoor.co.il |
| `italy` | Italy | glassdoor.it |
| `spain` | Spain | glassdoor.es |
| `sweden` | Sweden | glassdoor.se |

> **Note:** Israel (`glassdoor.co.il`) is temporarily unavailable due to Glassdoor infrastructure issues. The scraper will automatically fall back to `glassdoor.com` when this domain is down.

## Example Input

```
{
  "searchQuery": "Product Manager",
  "location": "New York",
  "country": "us",
  "maxItems": 50,
  "fetchDescriptions": true,
  "proxyConfig": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Sample Output

```
{
  "jobId": "1009428351674",
  "url": "https://www.glassdoor.com/job-listing/product-manager-acme-corp-JV_IC1132348_KO0,15_KE16,25.htm?jl=1009428351674",
  "jobTitle": "Product Manager",
  "employerName": "Acme Corp",
  "employerId": "28471",
  "location": "New York, NY",
  "jobType": "fulltime",
  "isRemote": false,
  "salaryMin": 120000,
  "salaryMedian": 145000,
  "salaryMax": 175000,
  "salaryCurrency": "USD",
  "salaryPeriod": "ANNUAL",
  "datePosted": "2026-03-24",
  "easyApply": true,
  "sponsored": false,
  "companyRating": 4.2,
  "companySize": "1001 to 5000 Employees",
  "companyIndustry": "Internet & Web Services",
  "listingDescription": "We are looking for a Product Manager to lead our platform team...",
  "scrapedAt": "2026-03-26T12:00:00.000Z"
}
```

## How It Works

The scraper uses Playwright (headless Chrome) to render Glassdoor pages, then extracts structured data from the Apollo GraphQL cache embedded in each page. This gives richer and more reliable data than HTML scraping alone.

## Keywords

Glassdoor scraper, Glassdoor jobs API, salary data scraper, job listings API, Glassdoor salary estimates, recruitment data, HR benchmarking tool, global job market data, Glassdoor companies, remote jobs scraper, glassdoor.com scraper, job posting data