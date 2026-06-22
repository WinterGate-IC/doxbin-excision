# INTELLIGENCE DOSSIER – DOXBIN INFRASTRUCTURE & BEHAVIOR

Compiled by WinterGate IC – Operation: Excision
Date: June 21, 2026
Classification: PUBLIC INTELLIGENCE

---

## 1. EXECUTIVE SUMMARY

This document provides a comprehensive technical analysis of the Doxbin platform – its origin infrastructure, subdomains, API endpoints, frontend-backend communication patterns, security protections, observed error responses, and infrastructure weaknesses. All findings are specific to Doxbin.

---

## 2. ORIGIN INFRASTRUCTURE

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

**Search & Content Endpoints:**
- /search – GET – Reflected XSS, SQL injection confirmed
- /api/search – GET – Vulnerable
- /api/index/pastes – GET – 403 Forbidden (returns HTML not JSON)
- /api/pastes – GET – Vulnerable
- /api/v1/pastes – GET – API v1 endpoint
- /api/v2/pastes – GET – API v2 endpoint

**User Interaction Endpoints:**
- /contact – POST – Stored XSS
- /submit – POST – Stored XSS
- /upload – POST – File upload (attempted)
- /post – POST – Paste creation
- /create – POST – Content creation
- /comment – POST – Comment system
- /feedback – POST – Feedback form
- /support – POST – Support form
- /help – POST – Help form

**Authentication & Profile Endpoints:**
- /login – GET/POST – Authentication
- /register – GET/POST – Registration
- /logout – GET – Logout
- /settings – POST – Profile defacement confirmed
- /users – GET – User enumeration
- /user/{id} – GET – Profile exposure

**Paste Endpoints:**
- /paste/{id} – GET – Paste content (public)
- /api/user/comment/create – POST – Stored XSS confirmed (4,940 injections)

**Admin & WordPress Endpoints:**
- /graphql – POST – GraphQL endpoint (403/405)
- /wp-admin/admin-ajax.php – POST – WordPress admin-ajax
- /wp-json/wp/v2/posts – GET – WordPress REST API

**Static & Asset Endpoints:**
- /static/* – GET – Static assets (CSS, JS, images)
- /static/inter.css – CSS file (MIME type errors)
- /static/base.css – CSS file (MIME type errors)
- /static/banner.jpg – Missing asset (404)
- /static/orange.gif – Missing asset (404)
- /static/default.jpg – Profile default image
- /favicon.ico – Missing asset (404)

**API & Utility Endpoints:**
- /api/timezone/set – POST – Timezone setting
- /api/notifications/read – POST – Notifications
- /api/token – POST – Token generation (403)
- /tos – GET – Terms of Service
- /hoa – GET – Hall of Autism
- /upgrade – GET – Upgrade page

**Cloudflare Endpoints:**
- /cdn-cgi/challenge-platform/* – Cloudflare challenge endpoints
- /cdn-cgi/turnstile/* – Cloudflare Turnstile

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

Deployed on:
- Login page
- Comment submission (/api/user/comment/create)
- Registration (/register)

Uses sitekey: 0x4AAAAAADgl1DWaCGN1UWZN
Renders in iframes with sandbox restrictions

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

Observations:
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

Endpoint: /api/index/pastes
Status: 403 Forbidden
Response: <!doctype html><html>... (HTML error page)
Interpretation: API endpoint is protected; unauthenticated requests return HTML error, not JSON.

Asset: /static/orange.gif
Status: 404 Not Found
Interpretation: Missing asset – incomplete deployment or outdated references.

Asset: /static/inter.css
Error: Refused to apply style because MIME type ('text/html') is not supported.
Interpretation: Server returns HTML error pages instead of CSS files – likely due to 403 or 500 errors.

Asset: /static/banner.jpg
Status: 404 Not Found
Interpretation: Missing asset.

Asset: /favicon.ico
Status: 404 Not Found
Interpretation: Missing asset.

Component: Turnstile (iframes)
Error: Blocked script execution in 'about:blank' because the document's frame is sandboxed and the 'allow-scripts' permission is not set.
Interpretation: Cloudflare's Turnstile sandbox restricts script execution – a defensive measure.

### 5.3 JavaScript Errors (Browser Console)

Error: Uncaught (in promise) SyntaxError: Unexpected token '<', "<!doctype "... is not valid JSON
Interpretation: API returned HTML instead of JSON – indicates server-side error or misconfiguration.

---

## 6. INFRASTRUCTURE WEAKNESSES

### 6.1 Input Validation Failures

- Reflected XSS on /search – unescaped input echoed in HTML
- Stored XSS on /api/user/comment/create – unsanitized comments stored in database
- SQL Injection on /search – error messages leak SQL syntax

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

## 7. CALL TO ACTION

1. Hosting Providers – Investigate and take appropriate action
2. Cloudflare – Review Doxbin's use of their services
3. Law Enforcement – Coordinate cross-jurisdictional takedown
4. Policymakers – Strengthen laws against doxing and cyber-harassment
5. Citizens & Journalists – Raise awareness

---

## 8. DISCLAIMER

This document is for educational and intelligence purposes only. All data is derived from publicly observable network behavior. The authors do not condone or encourage illegal activity.

---

## 9. WHAT A FREEZE

Documentation. Awareness. Accountability.

– WinterGate IC Command

Repository: https://github.com/WinterGate-IC/doxbin-excision
