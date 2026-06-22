# INTELLIGENCE DOSSIER – DOXBIN INFRASTRUCTURE & VULNERABILITIES

Compiled by WinterGate IC – Operation: Excision
Date: June 21, 2026
Classification: PUBLIC INTELLIGENCE

---

## 1. EXECUTIVE SUMMARY

Doxbin is a criminal platform enabling doxing, swatting, identity theft, and harassment. This dossier exposes the complete infrastructure, security vulnerabilities, and attack vectors identified during Operation: Excision. All findings are specific to Doxbin.

---

## 2. INFRASTRUCTURE MAPPING

### 2.1 Origin Servers (Real IPs)

- 185.53.179.200 – Germany – Primary origin server
- 185.53.179.145 – Germany – Secondary origin server
- 104.26.4.226 – Cloudflare – CDN/Proxy (frontend)
- 104.26.5.226 – Cloudflare – CDN/Proxy (frontend)

### 2.2 Subdomains Discovered

- www.doxbin.com
- mail.doxbin.com
- cpanel.doxbin.com
- whm.doxbin.com
- cp.doxbin.com
- ftp.doxbin.com
- ns1.doxbin.com
- ns2.doxbin.com
- ssh.doxbin.com
- git.doxbin.com
- api.doxbin.com
- admin.doxbin.com
- dashboard.doxbin.com
- panel.doxbin.com
- portal.doxbin.com

### 2.3 DNS Records

- A records point to Cloudflare IPs (104.26.4.226, 104.26.5.226)
- NS records indicate Cloudflare nameservers
- No direct A record for origin IPs – they are proxied

---

## 3. FRONTEND-BACKEND COMMUNICATION

### 3.1 API Endpoints (66 Total Mapped)

| Endpoint | Method | Purpose | Status |
|----------|--------|---------|--------|
| /search | GET | Search pastes | Reflected XSS, SQL injection confirmed |
| /api/search | GET | API search | Vulnerable |
| /api/index/pastes | GET | Fetch pastes | 403 Forbidden (returns HTML not JSON) |
| /api/pastes | GET | List pastes | Vulnerable |
| /api/v1/pastes | GET | API v1 | Vulnerable |
| /api/v2/pastes | GET | API v2 | Vulnerable |
| /contact | POST | Contact form | Stored XSS |
| /submit | POST | Submit form | Stored XSS |
| /upload | POST | File upload | Attempted |
| /post | POST | Paste creation | Stored XSS |
| /create | POST | Content creation | Stored XSS |
| /comment | POST | Comment system | Stored XSS |
| /feedback | POST | Feedback form | Stored XSS |
| /support | POST | Support form | Stored XSS |
| /help | POST | Help form | Stored XSS |
| /login | GET/POST | Authentication | Exposed |
| /register | GET/POST | Registration | Exposed |
| /logout | GET | Logout | Exposed |
| /settings | POST | Profile update | Profile defacement confirmed |
| /users | GET | User enumeration | Exposed |
| /user/{id} | GET | Profile exposure | Exposed |
| /paste/{id} | GET | Paste content (public) | Exposed |
| /api/user/comment/create | POST | Comment creation | Stored XSS confirmed (4,940 injections) |
| /graphql | POST | GraphQL | 403/405 |
| /wp-admin/admin-ajax.php | POST | WordPress admin-ajax | 403 |
| /wp-json/wp/v2/posts | GET | WordPress REST API | Exposed |
| /static/* | GET | Static assets | MIME type errors |
| /static/inter.css | GET | CSS file | MIME type error (returns HTML) |
| /static/base.css | GET | CSS file | MIME type error (returns HTML) |
| /static/banner.jpg | GET | Banner image | 404 Not Found |
| /static/orange.gif | GET | Image asset | 404 Not Found |
| /static/default.jpg | GET | Default profile image | 200 OK |
| /favicon.ico | GET | Favicon | 404 Not Found |
| /api/timezone/set | POST | Timezone setting | 200 OK |
| /api/notifications/read | POST | Notifications | 200 OK |
| /api/token | POST | Token generation | 403 Forbidden |
| /tos | GET | Terms of Service | 200 OK |
| /hoa | GET | Hall of Autism | 200 OK |
| /upgrade | GET | Upgrade page | 200 OK |
| /cdn-cgi/challenge-platform/* | GET | Cloudflare challenge | Cloudflare managed |
| /cdn-cgi/turnstile/* | GET | Cloudflare Turnstile | Cloudflare managed |

### 3.2 Communication Flow

1. Browser sends HTTPS requests to Cloudflare-proxied domain
2. Cloudflare forwards requests to origin servers (185.53.179.200/145)
3. Origin servers process requests and return HTML/JSON
4. On failure, origin returns HTML error pages (not JSON), often with stack traces

---

## 4. SECURITY PROTECTIONS (OBSERVED)

### 4.1 Cloudflare

- DDoS Mitigation: Actively blocks suspicious traffic (returns 403 challenges)
- Bot Management: Detects automated requests and serves Turnstile challenges
- Rate Limiting: Triggers 429 or 403 when threshold exceeded

### 4.2 Turnstile (Cloudflare CAPTCHA)

- Deployed on: Login page, Comment submission (/api/user/comment/create), Registration (/register)
- Sitekey: 0x4AAAAAADgl1DWaCGN1UWZN
- Renders in iframes with sandbox restrictions

### 4.3 Content Security Policy (CSP)

- default-src: 'none'
- script-src: 'nonce-...', 'unsafe-eval'
- style-src: 'unsafe-inline'
- img-src: 'self'
- connect-src: 'self'
- frame-src: 'self', blob:
- form-action: 'none'
- base-uri: 'self'
- trusted-types: xqUQ2, default

**Weaknesses:**
- unsafe-eval allowed (reduces CSP effectiveness)
- unsafe-inline for styles – potentially exploitable
- Nonce-based scripts – but nonce is static per page load

### 4.4 State-Table Firewalls

- Upstream routers maintain connection state tables
- Susceptible to exhaustion under sustained SYN floods
- Observed latency spikes >1000ms during peak attacks

---

## 5. OBSERVED ERRORS & RESPONSES

### 5.1 HTTP Status Codes

- 200 – Successful requests – Content served (HTML/JSON)
- 302 – Redirects – Login/logout, upgrade pages
- 403 – Forbidden – API endpoints, Turnstile challenges
- 404 – Not Found – Static assets (/static/orange.gif, /favicon.ico)
- 405 – Method Not Allowed – GraphQL (if POST not allowed)
- 429 – Too Many Requests – Rate limiting (rarely observed)

### 5.2 Specific Error Patterns

**Endpoint:** /api/index/pastes
- Status: 403 Forbidden
- Response: <!doctype html><html>... (HTML error page)
- Interpretation: API endpoint is protected; unauthenticated requests return HTML error, not JSON.

**Asset:** /static/orange.gif
- Status: 404 Not Found
- Interpretation: Missing asset – incomplete deployment or outdated references.

**Asset:** /static/inter.css
- Error: Refused to apply style because MIME type ('text/html') is not supported.
- Interpretation: Server returns HTML error pages instead of CSS files – likely due to 403 or 500 errors.

**Asset:** /static/banner.jpg
- Status: 404 Not Found
- Interpretation: Missing asset.

**Asset:** /favicon.ico
- Status: 404 Not Found
- Interpretation: Missing asset.

**Component:** Turnstile (iframes)
- Error: Blocked script execution in 'about:blank' because the document's frame is sandboxed and the 'allow-scripts' permission is not set.
- Interpretation: Cloudflare's Turnstile sandbox restricts script execution – a defensive measure.

### 5.3 JavaScript Errors (Browser Console)

- Error: Uncaught (in promise) SyntaxError: Unexpected token '<', "<!doctype "... is not valid JSON
- Interpretation: API returned HTML instead of JSON – indicates server-side error or misconfiguration.

---

## 6. VULNERABILITIES IDENTIFIED

### 6.1 Input Validation Failures

| Vulnerability | Endpoint | Status |
|---------------|----------|--------|
| Reflected XSS | /search | Confirmed |
| Stored XSS | /api/user/comment/create | Confirmed (4,940 injections) |
| SQL Injection | /search | Confirmed (error-based) |

### 6.2 API Inconsistency

- /api/index/pastes returns HTML on 403, not JSON
- Inconsistent error handling across endpoints

### 6.3 Session Management

- Tokens persist without rotation
- Tokens can be stolen via XSS and reused

### 6.4 Static Asset Security

- CSS files return HTML error pages – poor error handling
- Missing assets (orange.gif, banner.jpg, favicon.ico) – incomplete deployment

### 6.5 State-Table Exhaustion

- Upstream firewalls vulnerable to SYN floods
- No aggressive timeout or SYN cookies observed

### 6.6 Content Security Policy Weaknesses

- unsafe-eval allowed
- unsafe-inline for styles
- Nonce is static per page load

---

## 7. NETWORK LAYER VULNERABILITIES

| Vector | Success Rate | Impact |
|--------|--------------|--------|
| SYN Flood | 100% | State table exhaustion |
| SYN-ACK Flood | 100% | State table exhaustion |
| RST Storm | 100% | State table exhaustion |

**Impact:**
- State table exhaustion on upstream firewalls
- Denial of service for legitimate users
- Latency spikes >1000ms
- Cascading failure across multiple sub-brands

---

## 8. IMPACT ASSESSMENT

| Impact Area | Severity | Evidence |
|-------------|----------|----------|
| Service Availability | Critical | SYN floods achieved 100% success; state table exhaustion; latency >1000ms |
| Data Integrity | Critical | SQL injection enabled table drops, file writes, and sensitive file reads |
| Confidentiality | High | Session tokens, user IDs, and internal endpoints exposed |
| Reputation | High | Origin IPs exposed; domain blacklisted; multiple public breaches |

---

## 9. CALL TO ACTION

1. Hosting Providers – Investigate and take appropriate action
2. Cloudflare – Review Doxbin's use of their services
3. Law Enforcement – Coordinate cross-jurisdictional takedown
4. Policymakers – Strengthen laws against doxing and cyber-harassment
5. Citizens & Journalists – Raise awareness

---

## 10. DISCLAIMER

This document is for educational and intelligence purposes only. All data is derived from publicly observable network behavior. The authors do not condone or encourage illegal activity.

---

## 11. WHAT A FREEZE

Documentation. Awareness. Accountability.

– WinterGate IC Command

Repository: https://github.com/WinterGate-IC/doxbin-excision
