# APIVerve IP Intelligence Action

> Lookup IP geolocation, detect VPNs/proxies, and check IP reputation

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-IP_Intelligence-blue?logo=github)](https://github.com/apiverve/action-ip-intelligence)
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
| `iplookup` | IP Lookup resolves an IP address to its geographic location. It returns the country, region, city, coordinates, postal code, timezone and continent, along with an accuracy radius and an EU-membership flag for compliance routing. |
| `ipdemographics` | IP Demographics combines IP geolocation with Census demographic data to provide demographic information for any IP address. Get location, income, education, and housing data based on the IP's geographic location. Demographics cover US ZIP codes; other locations return the location with demographics set to null. |
| `vpndetector` | VPN Proxy Detector checks any IP address to detect active VPN connections and cloud datacenter hosting. Lookups return boolean flags for VPN and datacenter status plus the verification date, with paid tiers adding Tor detection and risk levels. |
| `tordetect` | Tor Node Detector checks whether an IP address belongs to an active Tor exit node. Send any IP address to verify parsing status and identify traffic originating from the anonymity network. |
| `ipblacklistlookup` | IP Blacklist Lookup checks whether a given IP address appears on known malicious IP blocklists. Identifies both inbound threats (attackers, spammers) and outbound threats (C2 servers, malware hosts). |
| `asnlookup` | ASN Lookup resolves any Autonomous System Number to its registered organization name and numeric identifier. Paid plans add the registry handle, country code, and announced IP range statistics. |

---

## Quick Start

```yaml
- name: IP Intelligence
  uses: apiverve/action-ip-intelligence@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: iplookup
    params: '{"ip": "8.8.8.8"}'
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
| `api` | API to use: `iplookup`, `ipdemographics`, `vpndetector`, `tordetect`, `ipblacklistlookup`, `asnlookup` | No | `iplookup` |
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
    params: '{"ip": "8.8.8.8"}'

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
    params: '{"ip": "8.8.8.8"}'

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
          params: '{"ip": "8.8.8.8"}'

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
