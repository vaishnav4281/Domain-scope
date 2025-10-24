# Domain Intelligence Toolkit - Overview

This document provides a high-level overview of the Domain Intelligence Toolkit, its features, architecture, and usage. For detailed guides, see the other docs in this folder.

## Features
- WHOIS & DNS lookup
- IP geolocation
- Abuse detection
- Metascraper metadata extraction (30+ fields)
- VirusTotal security analysis (70+ vendors)
- Bulk scanning
- CSV export
- Modern UI with dark mode

## Architecture
- Frontend: React + Vite + Tailwind CSS
- Backend: FastAPI (Python)
- APIs: WhoisXML, IP2Location, AbuseIPDB, VirusTotal
- CORS handled via multi-proxy fallback

## Usage
1. Enter a domain or IP
2. Click "Analyze"
3. View results in three cards: Backend, Metascraper, VirusTotal
4. Export or copy results as needed

See the other docs for setup, API details, and troubleshooting.