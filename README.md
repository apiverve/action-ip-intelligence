# APIVerve IP Intelligence Action

> Lookup IP geolocation, detect VPNs/proxies, and check IP reputation

> **Beta Release** - This action is in beta. We'd love your feedback! [Open an issue](https://github.com/apiverve/action-ip-intelligence/issues) if you encounter any problems.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-IP Intelligence-blue?logo=github)](https://github.com/marketplace/actions/apiverve-ip-intelligence)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence)**

---

## What does this action do?

This action provides access to APIVerve's IP Intelligence APIs directly in your GitHub workflows:

- Geolocate IP addresses
- Detect VPN and proxy usage
- Check if IPs are on blacklists
- Get ASN information for network analysis

### Available APIs

| API | Description |
|-----|-------------|
| `iplookup` | IP Lookup is a simple tool for looking up the location of an IP address. It returns the country, city, and more. |
| `ipdemographics` | IP Demographics combines IP geolocation with Census demographic data to provide demographic information for any IP address. Get location, income, education, and housing data based on the IP&#x27;s geographic location. |
| `vpndetector` | VPN Detector is a simple tool for detecting VPN usage. It returns a boolean value indicating whether the IP address is using a VPN or not. |
| `tordetector` | tordetector API |
| `ipblacklistlookup` | IP Blacklist Lookup checks whether a given IP address appears on known malicious IP blocklists. Identifies both inbound threats (attackers, spammers) and outbound threats (C2 servers, malware hosts). |
| `asnlookup` | ASN Lookup is a simple tool for getting information on Autonomous System Numbers (ASNs). It returns information on various ASNs. |

---

## Quick Start

```yaml
- name: IP Intelligence
  uses: apiverve/action-ip-intelligence@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: iplookup
    params: '{&quot;ip&quot;: &quot;8.8.8.8&quot;}'
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: IP Intelligence
  uses: apiverve/action-ip-intelligence@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: iplookup
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `iplookup`, `ipdemographics`, `vpndetector`, `tordetector`, `ipblacklistlookup`, `asnlookup` | No | `iplookup` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |

*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |

---

## Examples

### IP Geolocation

Get location information for an IP address

```yaml
- name: IP Geolocation
  id: ip-intelligence-0
  uses: apiverve/action-ip-intelligence@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: iplookup
    params: '{&quot;ip&quot;: &quot;8.8.8.8&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.ip-intelligence-0.outputs.data }}"
```

### VPN Detection

Check if an IP is a VPN or proxy

```yaml
- name: VPN Detection
  id: ip-intelligence-1
  uses: apiverve/action-ip-intelligence@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: vpndetector
    params: '{&quot;ip&quot;: &quot;8.8.8.8&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.ip-intelligence-1.outputs.data }}"
```


---

## Full Workflow Example

```yaml
name: IP Intelligence Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  ip-intelligence:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run IP Intelligence
        id: result
        uses: apiverve/action-ip-intelligence@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: iplookup
          params: '{&quot;ip&quot;: &quot;8.8.8.8&quot;}'

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-ip-intelligence/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=ip-intelligence) - 350+ APIs for developers
