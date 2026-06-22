[Glassdoor Scraper](https://apify.com/vivid_astronaut/glassdoor-scraper?fpr=data)

Glassdoor Scraper service powered by Azure. Fast, reliable, and scalable API.

## Features

- **Fast Processing**: Lightning-fast glassdoor scraper api powered by Azure
- **Reliable**: 99.9% uptime with automatic failover
- **Scalable**: Handle single requests or bulk operations
- **Secure**: Enterprise-grade security with API key authentication
- **Well Documented**: Comprehensive API documentation and examples

## Use Cases

- **Development**: Integrate into your development workflow
- **Automation**: Build automated pipelines
- **Integration**: Connect with other services

## Input Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `data` | object | No | Input data to process. Send your API request payload here. |
| `endpoint` | string | No | Specific endpoint to call (e.g., /api/v1/process) |

## Output Format

```
{
  "success": true,
  "result": { ... },
  "timestamp": "2026-01-07T00:00:00Z"
}
```

## Code Examples

### JavaScript (Node.js)

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });

const input = {
  "data": {},
  "endpoint": "/api/v1/process"
};

const run = await client.actor("vivid_astronaut/glassdoor-scraper").call(input);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")

run_input = {
  "data": {},
  "endpoint": "/api/v1/process"
}

run = client.actor("vivid_astronaut/glassdoor-scraper").call(run_input=run_input)

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### cURL

```
curl -X POST "https://api.apify.com/v2/acts/vivid_astronaut~glassdoor-scraper/runs?token=YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
  "data": {},
  "endpoint": "/api/v1/process"
}'
```

## Pricing

**Model**: Pay per result
**Price**: $0.010 per result

You only pay for successful results. Platform usage costs are included.

## API Documentation

Full API documentation is available at:

- [Apify API Reference](https://docs.apify.com/api/v2)
- [Actor API Endpoints](https://docs.apify.com/api/v2#/reference/actors)

## Support

- **Issues**: Report bugs via Apify Console
- **Documentation**: [Apify Docs](https://docs.apify.com)
- **Community**: [Apify Discord](https://discord.gg/apify)

## Version History

See ./CHANGELOG.md for version history.

---

*Powered by Azure Cloud Infrastructure*