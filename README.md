[Glassdoor Scraper](https://apify.com/ahmed_jasarevic/glassdoor-scraper?fpr=data)

# Glassdoor Scraper Pro 🚀

A high-performance, production-ready Apify Actor designed to extract comprehensive data from Glassdoor.com while bypassing aggressive anti-bot protections.

## ✨ Key Features

- **🛡️ Anti-Bot & Cloudflare Bypass**: Specialized Playwright Stealth engine.
- **🍪 Automated Session Persistence (New!)**: Save your session once, and the Actor remembers it for next time!
- **🛡️ Hidden Data Recovery**: Extracts "blurred" reviews and hidden text.
- **📡 Hybrid Extraction Engine**:

- **GraphQL Interception**: Captures high-fidelity JSON data directly from internal API calls.
- **DOM Recovery**: Intelligent fallback that scrapes visible cards, including **Blurred/Hidden** review text.
- **💾 Real-time Results**: Results appear in the dataset instantly as they are scraped.

## 🍪 Mandatory Cookie Setup (Cloud Success)

To bypass Glassdoor's Cloudflare protection on the Apify Platform, you **must** provide fresh cookies from your own logged-in browser session.

### How to get your cookies:

1. Open Glassdoor in your browser and ensure you are logged in.
2. Use a browser extension like **"EditThisCookie"** or **"JSON Cookie"** to export your cookies as a JSON array.
3. Alternatively, open **Developer Tools (F12)** -> **Network** tab -> Refresh -> Right-click any request -> **Copy** -> **Copy as JSON**.
4. Paste the JSON array into the **`cookies`** input field in the Apify Console.

> [!TIP]
> You only need to provide cookies **once**! After the first successful run, the Actor will automatically save its session state. Subsequent runs will load this saved session from the Key-Value Store.

## ⚙️ Configuration

| Field | Type | Description |
| --- | --- | --- |
| `startUrls` | Array | List of Glassdoor URLs (Reviews, Jobs, etc.) |
| `maxItems` | Number | Maximum items to collect per URL (Default: 100) |
| `command` | String | Data type to target: `reviews`, `jobs`, `salaries` |
| `cookies` | Array | Your session cookies (JSON format) |
| `proxy` | Object | **MANDATORY**: Use Residential Proxy for Cloud runs. |

## 📦 Output

Data is saved to `storage/datasets/default/`. Each record includes:

- Review ID & Summary
- Pros & Cons (Full Text)
- Ratings (Overall & Sub-ratings)
- Metadata (Date, Location, Job Title)
- `_source`: Traceability for where the data was captured (GraphQL vs DOM)