# VirusTotal Integration

## Overview
- Uses VirusTotal API v3 for domain intelligence
- Fetches: reputation, security scores, categories, DNS, SSL, tags, vendor results
- Displays in a tabbed card with risk level

## Setup
- Get API key from https://www.virustotal.com/
- Add to `.env` as `VITE_VIRUSTOTAL_API_KEY`
- Restart dev server

## Features
- 70+ vendor detections
- Community reputation
- DNS/SSL info
- Risk level calculation
- Categories/tags

## Rate Limits
- Free: 4 requests/min, 500/day
- Premium: higher limits

## Troubleshooting
- Invalid key: check `.env`
- Rate limit: wait and retry
- See `docs/troubleshooting.md`