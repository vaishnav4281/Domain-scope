# VirusTotal Integration Guide

## Overview

The Domain Intelligence Toolkit now integrates with **VirusTotal API v3** to provide comprehensive security analysis and threat intelligence for domains. This integration adds a third results panel displaying security scores, reputation data, DNS records, SSL certificates, and detection results from 70+ antivirus engines.

## Features

### Security Analysis
- **Malicious Score**: Number of security vendors flagging the domain as malicious
- **Suspicious Score**: Number of vendors reporting suspicious activity
- **Harmless Score**: Vendors confirming the domain is clean
- **Undetected Score**: Vendors with no detection
- **Risk Level**: Automated assessment (Clean/Low/Medium/High) based on threat scores

### Reputation Intelligence
- **Reputation Score**: Community-driven reputation rating
- **Community Votes**: Harmless vs Malicious votes from VirusTotal community
- **Popularity Rankings**: Domain rankings from various traffic analysis services (Alexa, Cisco Umbrella, etc.)
- **Categories**: Domain categorization by security vendors (e.g., "news", "social media", "phishing")

### DNS & SSL Data
- **DNS Records**: Complete DNS record history (A, AAAA, MX, NS, TXT, CNAME, etc.)
- **SSL Certificates**: Certificate details including issuer, subject, validity dates
- **JARM Fingerprint**: TLS/SSL server fingerprint for infrastructure analysis

### Domain Information
- **WHOIS Data**: Domain registration information from VirusTotal's database
- **Creation Date**: When the domain was first registered
- **Last Update**: Most recent domain update timestamp
- **Registrar**: Domain registrar information
- **Tags**: Security-related tags applied by analysts

### Vendor Detection Results
- **70+ Security Vendors**: Individual detection results from major antivirus engines
- **Detection Categories**: Categorized results (malicious, suspicious, harmless, undetected)
- **Threat Names**: Specific threat classifications from each vendor

## Getting Your API Key

### Step 1: Create VirusTotal Account
1. Visit [https://www.virustotal.com/](https://www.virustotal.com/)
2. Click **Sign Up** in the top right corner
3. Register with your email or use Google/GitHub authentication
4. Verify your email address

### Step 2: Get API Key
1. Log in to your VirusTotal account
2. Click on your **profile icon** (top right)
3. Select **API Key** from the dropdown menu
4. Copy your API key (it will look like: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6`)

### Step 3: Add API Key to Project
1. Open the `.env` file in your project root
2. Replace `your_api_key_here` with your actual API key:
   ```
   VITE_VIRUSTOTAL_API_KEY=a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6
   ```
3. Save the file and restart your development server

## API Rate Limits

### Free Tier (Public API)
- **Request Rate**: 4 requests per minute
- **Daily Quota**: 500 requests per day
- **Monthly Quota**: 15,500 requests per month

### Premium Tier
- **Request Rate**: Up to 1,000 requests per minute (depending on plan)
- **Daily Quota**: Unlimited
- **Additional Features**: Private scanning, YARA rules, advanced threat intelligence

**Note**: The free tier is sufficient for most OSINT investigations. If you exceed the rate limit, the application will display an error message and you'll need to wait before scanning again.

## Understanding the Data

### Risk Level Calculation
The application automatically calculates risk levels based on detection scores:

- **Clean**: 0 malicious + 0 suspicious detections
- **Low Risk**: 1-2 malicious OR 1-3 suspicious detections
- **Medium Risk**: 3-5 malicious OR 4-10 suspicious detections
- **High Risk**: 6+ malicious OR 11+ suspicious detections

### What Each Tab Shows

#### Security Tab
- Real-time threat detection statistics
- Color-coded scores (red=malicious, orange=suspicious, green=harmless, gray=undetected)
- Community voting results

#### Detection Tab
- Complete list of all 70+ antivirus vendors and their verdicts
- Threat classification for each positive detection
- Easy identification of consensus among security vendors

#### Reputation Tab
- Domain popularity rankings from traffic analysis services
- Helps identify legitimate high-traffic domains vs. suspicious low-traffic ones
- Last analysis timestamp

#### Categories Tab
- Domain categorization by multiple security vendors
- Common categories: "news", "social networking", "phishing", "malware", etc.
- Registrar information

#### DNS/SSL Tab
- Complete DNS record history (useful for tracking domain infrastructure changes)
- SSL certificate details (validates HTTPS security)
- JARM fingerprint (identifies server/CDN infrastructure)

#### Info Tab
- Domain registration dates (creation, last update)
- WHOIS information snapshot from VirusTotal
- Useful for identifying newly registered domains (often suspicious)

## Example Use Cases

### 1. Phishing Investigation
**Scenario**: User receives suspicious email from `secure-bank-login.com`

**What to check**:
- **Security Tab**: High malicious/suspicious scores indicate known phishing
- **Categories**: Look for "phishing" categorization
- **Info Tab**: Newly created domains (< 30 days) are red flags
- **Detection Tab**: Check if major vendors (Google Safe Browsing, Fortinet) flag it

### 2. Malware Distribution Site
**Scenario**: Investigating `download-free-software.xyz`

**What to check**:
- **Detection Tab**: Look for "malware" or "trojan" classifications
- **DNS/SSL Tab**: Suspicious domains often lack proper SSL or use free certificates
- **Reputation**: Low/negative reputation score
- **Categories**: "malicious content" or "malware distribution"

### 3. Domain Reputation Analysis
**Scenario**: Verifying legitimacy of `microsoft-support.com`

**What to check**:
- **Reputation Tab**: Legitimate domains have positive votes and high rankings
- **Categories**: Should match expected category (e.g., "technology")
- **Info Tab**: Long registration history vs. newly created lookalike
- **DNS/SSL Tab**: Legitimate companies use proper SSL certificates from trusted CAs

### 4. Infrastructure Tracking
**Scenario**: Monitoring threat actor infrastructure

**What to check**:
- **DNS Tab**: Track IP changes over time
- **JARM Fingerprint**: Identify similar server configurations
- **SSL Tab**: Shared certificates across multiple malicious domains
- **Tags**: Look for analyst-applied tags like "apt", "ransomware", etc.

## API Endpoint Details

The integration uses the VirusTotal API v3 domain intelligence endpoint:

```
GET https://www.virustotal.com/api/v3/domains/{domain}
```

**Headers Required**:
- `x-apikey`: Your VirusTotal API key

**Response Data** (abbreviated):
```json
{
  "data": {
    "attributes": {
      "reputation": -50,
      "last_analysis_stats": {
        "malicious": 12,
        "suspicious": 3,
        "harmless": 60,
        "undetected": 15
      },
      "total_votes": {
        "harmless": 25,
        "malicious": 8
      },
      "categories": {
        "Fortinet": "malicious",
        "Google": "phishing"
      },
      "popularity_ranks": {
        "Alexa": {
          "rank": 15000
        }
      },
      "last_dns_records": [...],
      "last_https_certificate": {...},
      "whois": "...",
      "creation_date": 1234567890,
      "tags": ["phishing", "spam"]
    }
  }
}
```

## Troubleshooting

### "Invalid API Key" Error
- Verify you copied the entire API key correctly (should be 64 characters)
- Check for extra spaces in the `.env` file
- Ensure the variable name is exactly `VITE_VIRUSTOTAL_API_KEY`
- Restart your development server after changing `.env`

### "Rate Limit Exceeded" Error
- You've made more than 4 requests per minute (free tier)
- Wait 60 seconds before trying again
- Consider upgrading to a premium API plan for higher limits
- Use bulk scanning sparingly

### "Domain Not Found" Error
- The domain may not exist in VirusTotal's database yet
- Newly registered domains may take time to appear
- Try submitting the domain manually on virustotal.com first

### No Data Displayed
- Check browser console (F12) for errors
- Verify the API key is set in `.env`
- Ensure you've restarted the development server
- Check if the domain exists and has been scanned by VirusTotal

### CORS Errors
- VirusTotal API v3 should not have CORS issues when called from backend
- If you see CORS errors, the request might be failing before reaching VirusTotal
- Check API key validity first

## Privacy & Security Considerations

### Data Sharing
When you submit a domain to VirusTotal:
- The domain becomes part of VirusTotal's public database
- Other users can see the scan results
- Your API key identifies your requests (but not publicly)

### Best Practices
- **Never** commit your API key to version control (it's in `.gitignore`)
- Don't scan sensitive internal domains that shouldn't be public
- Rotate your API key if you suspect it's been compromised
- Use environment variables to keep API keys secure

### Data Retention
- VirusTotal stores scan results indefinitely
- Historical data helps track domain reputation over time
- Old scans can be useful for incident response and forensics

## Advanced Features

### Filtering Detection Results
The component shows all 70+ vendor results, but you can focus on:
- **Major Vendors**: Google Safe Browsing, Microsoft, Kaspersky, Symantec
- **Malicious Only**: Filter to show only positive detections
- **Consensus**: Look for consistent categorization across multiple vendors

### Comparing Results Over Time
- Save screenshots of VirusTotal results
- Re-scan domains periodically to track reputation changes
- Newly registered domains often get flagged later as threat intelligence improves

### Export Functionality
While not currently implemented, you could extend the component to:
- Export VirusTotal results to CSV/JSON
- Generate PDF reports combining WHOIS + Metascraper + VirusTotal data
- Create custom risk assessment reports

## API Documentation

For complete API documentation, visit:
- **Official Docs**: https://developers.virustotal.com/reference/overview
- **Domain Endpoint**: https://developers.virustotal.com/reference/domain-info
- **Authentication**: https://developers.virustotal.com/reference/authentication

## Support

If you encounter issues:
1. Check this documentation first
2. Review VirusTotal's official API docs
3. Check the browser console for detailed error messages
4. Verify your API key is active and has quota remaining
5. Test the API key directly using curl:
   ```bash
   curl --request GET \
     --url https://www.virustotal.com/api/v3/domains/google.com \
     --header 'x-apikey: YOUR_API_KEY'
   ```

---

**Built with ❤️ for the cybersecurity community**
*Always use threat intelligence responsibly and ethically.*
