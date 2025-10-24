# API Reference

## Backend Endpoints
- `/whois/?domain=`: WHOIS info
- `/ipgeo/?ip=`: IP geolocation
- `/abuse/?ip=`: AbuseIPDB risk
- `/dns/?domain=`: DNS records

## VirusTotal API
- Endpoint: `https://www.virustotal.com/api/v3/domains/{domain}`
- Requires `x-apikey` header
- Returns: reputation, analysis stats, categories, DNS, SSL, tags, vendor results

## Metascraper
- Client-side HTML fetch via proxy
- Extracts: title, description, images, social cards, JSON-LD, feeds, etc.

## Bulk Scanner
- Accepts CSV or list of domains
- Returns batch results

See `docs/features.md` for more details.