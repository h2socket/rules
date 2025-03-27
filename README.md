
# 🛡️ Simple Cloudflare Firewall Rules by h2socket

Level up your site protection using Cloudflare’s custom WAF rules. These rules are tailored for maximum security with minimal user impact. Plug and play them into your dashboard.

---

## 🔒 1. Invisible Challenge for All (Excluding Bots)

```
(not cf.client.bot and ip.src ne 100.64.0.0/10)
```
**Action:** Challenge (Managed Challenge)  
**Use:** Presents a Turnstile/invisible challenge to human visitors.

---

## 🚫 2. Block Browser Emulations & Tools

```
(http.user_agent contains "Headless" or 
 http.user_agent contains "python" or 
 http.user_agent contains "curl" or 
 http.user_agent contains "wget" or 
 http.user_agent eq "" or 
 http.headers["sec-ch-ua"] eq "")
```
**Action:** Block  
**Use:** Blocks known automation tools and incomplete headers.

---

## 🧠 3. Headless & Suspicious Client Detection

```
(js.challenge_failed or 
 not cf.client.bot and 
 (http.request.version in {"1.0" "0.9"} or
 http.headers["accept-language"][0] eq "" or
 http.headers["user-agent"] eq ""))
```
**Action:** Challenge (Managed Challenge)  
**Use:** Catches advanced bots or malformed requests.

---

## 🌊 4. Basic Pre-Firewall Geo Flood Filtering

```
(ip.geoip.country in {"CN" "RU" "KP"} and 
 http.request.uri.path contains "/") or 
 cf.threat_score gt 20
```
**Action:** JS Challenge  
**Use:** Stops early-stage bot floods using threat score and location.

---

## 🚨 5. Emergency Lockdown (Login Pages)

```
(http.request.uri.path contains "/login" or 
 http.request.uri.path contains "/{YOUR_ADMIN_PATH}") and 
 ip.src not in {YOUR_ADMIN_IP}
```
**Action:** Block or Challenge  
**Use:** Secures sensitive endpoints. Replace `{YOUR_ADMIN_IP}` with yours and {YOUR_ADMIN_PATH} too (/admin or /wp-login.php etc).

---

## ⏱️ 6. Rate Limiting Rule

**Settings:**
- **Path**: `/*`
- **Threshold**: 30 requests / 10 seconds per IP
- **Action**: Challenge or Block

Enable under **Security → WAF → Rate Limiting Rules**.

---

## 📡 AbuseIPDB Integration (Optional)

Export logs via Cloudflare Logpush, then automate IP reporting to [AbuseIPDB](https://www.abuseipdb.com/) using their API. Ask for a script if needed.

---

## ✅ How to Add These

Go to:  
`Cloudflare Dashboard → Security → WAF → Custom Rules → Create rule`  
Paste the expression and assign an action.

---

Made with ☕ by **h2socket**  
Feel free to fork, remix, and deploy 🚀
