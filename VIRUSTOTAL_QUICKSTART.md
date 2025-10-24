# VirusTotal Integration - Quick Start

## 🚀 5-Minute Setup

### 1. Get Your API Key (2 minutes)
1. Go to https://www.virustotal.com/
2. Sign up/login
3. Click your profile → **API Key**
4. Copy the key

### 2. Add to Project (1 minute)
```bash
# Edit .env file
VITE_VIRUSTOTAL_API_KEY=paste_your_key_here
```

### 3. Restart Server (30 seconds)
```bash
# Stop the dev server (Ctrl+C)
npm run dev
```

### 4. Test It! (1 minute)
1. Open http://localhost:5173
2. Enter a domain (e.g., `google.com`)
3. Click **Analyze Domain**
4. See the **VirusTotal Security Analysis** panel appear!

---

## ✅ What You'll See

### ✨ 6 Information Tabs

1. **🛡️ Security** - Malicious/Suspicious/Harmless scores (color-coded!)
2. **🔍 Detection** - 70+ antivirus vendor results
3. **⭐ Reputation** - Community votes & popularity rankings
4. **📁 Categories** - Domain classification by vendors
5. **🌐 DNS/SSL** - DNS records & SSL certificates
6. **ℹ️ Info** - Creation date, WHOIS, registrar

### 🎨 Risk Levels (Auto-calculated)
- 🟢 **Clean** - No threats detected
- 🟡 **Low** - 1-2 malicious detections
- 🟠 **Medium** - 3-5 malicious detections
- 🔴 **High** - 6+ malicious detections

---

## 📊 Example Domains to Test

| Domain | Expected Result | What to Look For |
|--------|-----------------|------------------|
| `google.com` | ✅ Clean | High reputation, many harmless votes |
| `microsoft.com` | ✅ Clean | Legitimate SSL, long registration history |
| `test-phishing.com` | ⚠️ May vary | Check detection tab for phishing flags |

---

## ⚠️ Free Tier Limits

- **4 requests/minute**
- **500 requests/day**

💡 **Tip**: Wait 15 seconds between scans to avoid rate limiting!

---

## 🐛 Troubleshooting

### "Invalid API Key"
- Copy the **entire** key (should be 64 characters)
- No extra spaces in `.env` file
- Restart server after editing `.env`

### "Rate Limit Exceeded"
- Wait 60 seconds and try again
- You've made more than 4 requests in the last minute

### No Results Showing
- Open browser console (F12) to check for errors
- Verify API key is in `.env`
- Domain might not be in VirusTotal database yet

---

## 📚 Full Documentation

See [VIRUSTOTAL_INTEGRATION.md](VIRUSTOTAL_INTEGRATION.md) for:
- Detailed API explanations
- Advanced use cases
- Understanding threat intelligence
- Security best practices

---

**Ready to enhance your domain intelligence? 🚀**
