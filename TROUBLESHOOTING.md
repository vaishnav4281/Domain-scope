# Troubleshooting Guide - Metascraper Implementation

## Common Issues & Solutions

### ❌ Error: "Failed to fetch"

**Problem**: This error occurs when trying to fetch HTML from other domains due to CORS (Cross-Origin Resource Sharing) restrictions.

**Solution Applied**: ✅
- The code now uses a **CORS proxy** service: `allorigins.win`
- This proxy fetches the HTML on your behalf and returns it without CORS restrictions
- Format: `https://api.allorigins.win/raw?url={targetDomain}`

**Code Change**:
```typescript
// ❌ Old (causes CORS error):
const response = await fetch(`https://${domain}`);

// ✅ New (uses CORS proxy):
const corsProxyUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(`https://${domain}`)}`;
const response = await fetchWithTimeout(corsProxyUrl, 10000);
```

---

### ⏱️ Request Timeout

**Symptom**: Error message says "Request timed out while fetching metadata"

**Cause**: The target website is slow to respond or the CORS proxy is overloaded

**Solution**:
- Default timeout is set to 10 seconds
- This is normal for some websites
- The backend WHOIS results will still work
- Error is displayed gracefully in the Metascraper card

---

### 🚫 Some Metadata Missing

**Symptom**: Metascraper card shows some fields but not others (e.g., no author, no image)

**Cause**: The target website doesn't include those meta tags in their HTML

**This is normal!** Not all websites have:
- Author tags
- Open Graph images
- Publisher information
- Publication dates

**Example**:
- ✅ `github.com` - Has rich metadata (title, description, image, logo)
- ⚠️ `example.com` - Minimal metadata (just title and description)
- ❌ `some-small-site.com` - May only have a title

---

### 🔒 CORS Proxy Alternatives

If `allorigins.win` is down or slow, you can try these alternatives:

#### Option 1: CORS Anywhere (requires setup)
```typescript
const corsProxyUrl = `https://cors-anywhere.herokuapp.com/${targetUrl}`;
```
Note: Requires requesting temporary access

#### Option 2: AllOrigins (current - recommended)
```typescript
const corsProxyUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(targetUrl)}`;
```

#### Option 3: Build your own backend proxy
Create an API endpoint on your backend:
```javascript
// Backend endpoint
app.get('/api/proxy', async (req, res) => {
  const { url } = req.query;
  const response = await fetch(url);
  const html = await response.text();
  res.send(html);
});
```

Then use it:
```typescript
const response = await fetch(`/api/proxy?url=${encodeURIComponent(targetUrl)}`);
```

---

### 🌐 Website Blocking Scraping

**Symptom**: Some websites return errors or empty content

**Cause**: The website detects scraping and blocks it

**Affected Sites**:
- Sites with aggressive anti-bot protection (Cloudflare, etc.)
- Sites requiring authentication
- Single-page apps (SPAs) that render content with JavaScript

**Solution**:
- Backend WHOIS data still works (not affected)
- Metascraper will show an error but won't break the app
- Consider using their official API if available

---

### 📱 Mobile/Network Issues

**Symptom**: Works on desktop but fails on mobile

**Possible Causes**:
1. Mobile network blocking CORS proxy
2. Slower connection causing timeouts
3. Cellular provider restrictions

**Solutions**:
- Try on WiFi instead of cellular
- Increase timeout (currently 10 seconds)
- Some networks may require VPN

---

### 🔧 How to Change CORS Proxy

If you want to switch to a different CORS proxy service:

1. Open `src/components/DomainAnalysisCard.tsx`
2. Find this line (around line 123):
```typescript
const corsProxyUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(targetUrl)}`;
```
3. Replace with your preferred proxy:
```typescript
// Example alternatives:
const corsProxyUrl = `https://corsproxy.io/?${encodeURIComponent(targetUrl)}`;
// OR
const corsProxyUrl = `https://api.codetabs.com/v1/proxy?quest=${encodeURIComponent(targetUrl)}`;
```

---

### 🎯 Testing Metascraper

**Good Test Domains** (rich metadata):
- ✅ `github.com`
- ✅ `stackoverflow.com`
- ✅ `medium.com`
- ✅ `bbc.com`
- ✅ `youtube.com`
- ✅ `reddit.com`

**Limited Metadata Domains**:
- ⚠️ `example.com` (minimal)
- ⚠️ `localhost` (won't work)
- ⚠️ Internal/private domains (inaccessible)

---

### 📊 What Should Work

After the fix:

| Feature | Status | Notes |
|---------|--------|-------|
| Backend WHOIS Results | ✅ Always works | Not affected by CORS |
| Metascraper with CORS proxy | ✅ Works for most sites | May timeout on slow sites |
| Error handling | ✅ Graceful | Shows friendly error messages |
| Timeout protection | ✅ 10 seconds | Prevents hanging |
| Mobile compatibility | ✅ Works | May need WiFi |

---

### 🐛 Still Having Issues?

**Check the browser console**:
1. Press `F12` to open DevTools
2. Go to "Console" tab
3. Look for errors (red text)
4. Check "Network" tab to see failed requests

**Common console errors and fixes**:

```
❌ "Failed to fetch"
✅ Fixed by CORS proxy (already implemented)

❌ "net::ERR_BLOCKED_BY_CLIENT"
✅ Disable ad blockers/privacy extensions

❌ "TypeError: Failed to fetch"
✅ Check internet connection

❌ "AbortError: The operation was aborted"
✅ Request timed out (normal for slow sites)
```

---

### 💡 Best Practices

1. **Always test with known-good domains first** (like github.com)
2. **Backend results are primary** - Metascraper is supplementary
3. **Expect some failures** - Not all sites are scrapable
4. **Check console for details** - Errors provide helpful information
5. **Be patient** - Some sites take time to fetch

---

### 🚀 Performance Tips

1. **Reduce timeout** for faster failures:
```typescript
const response = await fetchWithTimeout(corsProxyUrl, 5000); // 5 seconds
```

2. **Add retry logic**:
```typescript
let retries = 3;
while (retries > 0) {
  try {
    const response = await fetchWithTimeout(corsProxyUrl, 10000);
    break; // Success
  } catch (err) {
    retries--;
    if (retries === 0) throw err;
  }
}
```

3. **Cache results** (for repeated scans):
```typescript
const cache = new Map();
if (cache.has(domain)) {
  return cache.get(domain);
}
```

---

## Summary

The **"Failed to fetch"** error has been fixed by:
1. ✅ Using CORS proxy (`allorigins.win`)
2. ✅ Adding proper error handling
3. ✅ Implementing timeout protection
4. ✅ Showing user-friendly error messages

The app should now work correctly! If you still see errors, they're likely:
- Specific websites blocking scraping (normal)
- Network/timeout issues (temporary)
- CORS proxy being slow (try different domain)

The backend WHOIS analysis will **always work** regardless of Metascraper issues! 🎉
