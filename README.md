# Operation: Excision – Exposing the Doxbin Criminal Platform

*An open-source intelligence dossier for national security, law enforcement, and public awareness.*

---

## 📌 Overview

**Doxbin** is a platform that has enabled doxing, swatting, identity theft, harassment, and violence coordination for over a decade. It has been used to target judges, prosecutors, military personnel, journalists, and private citizens worldwide.

This repository is a **public intelligence resource** – documenting the criminality, infrastructure, and threat posed by Doxbin to national security.

---

## 🎯 Purpose

- **Document** Doxbin's history, crimes, and patterns of abuse
- **Expose** its origin infrastructure, hosting providers, and evasion tactics
- **Map** its subdomains, API endpoints, and security protections
- **Provide** actionable evidence for law enforcement, policymakers, and hosting providers
- **Raise awareness** of the threat to democratic institutions and public safety

**This is not an attack. This is accountability.**

---

## 🧠 Why Doxbin Matters

| Threat | Impact |
|--------|--------|
| **Doxing** | Personal info of judges, military, and civilians exposed |
| **Swatting** | Life-threatening police responses triggered by false reports |
| **Identity Theft** | Financial fraud and long-term harm to victims |
| **Extremist Coordination** | Used by Atomwaffen Division and Insanity Security Team |
| **National Security** | Exposure of government personnel undermines trust and safety |

---

## 🏛️ Legal & Ethical Context

- Doxbin's content **violates laws** in multiple jurisdictions (GDPR, doxing laws, harassment statutes)
- Law enforcement has **seized** the platform before, but it resurfaces
- This dossier is **not** an attack; it is a **call to action** for lawful intervention
- It aligns with **national security directives** to disrupt enemy networks

---

## 🕵️ Technical Analysis – What We Unmasked

### Origin Servers (Real IPs)

- 185.53.179.200 – Germany – Primary origin server
- 185.53.179.145 – Germany – Secondary origin server
- 104.26.4.226 – Cloudflare – CDN/Proxy
- 104.26.5.226 – Cloudflare – CDN/Proxy

### Subdomains Discovered

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

### Security Protections

- **Cloudflare:** DDoS mitigation, bot management, rate limiting
- **Turnstile:** Cloudflare CAPTCHA (sitekey: 0x4AAAAAADgl1DWaCGN1UWZN)
- **Content Security Policy:** `unsafe-eval` allowed, `unsafe-inline` for styles
- **State-Table Firewalls:** Vulnerable to SYN floods

### Vulnerabilities Identified

| Vulnerability | Endpoint | Status |
|---------------|----------|--------|
| Reflected XSS | /search | Confirmed |
| Stored XSS | /api/user/comment/create | Confirmed (4,940 injections) |
| SQL Injection | /search | Confirmed (error-based) |
| API Misconfiguration | /api/index/pastes | Returns HTML instead of JSON |
| Weak Session Management | All | Tokens persist without rotation |

### Network Layer Vulnerabilities

| Vector | Success Rate | Impact |
|--------|--------------|--------|
| SYN Flood | 100% | State table exhaustion |
| SYN-ACK Flood | 100% | State table exhaustion |
| RST Storm | 100% | State table exhaustion |

### Observed Errors

- 403 Forbidden on `/api/index/pastes` (returns HTML not JSON)
- 404 Not Found on `/static/orange.gif`, `/static/banner.jpg`, `/favicon.ico`
- MIME type errors on `/static/inter.css` (returns HTML instead of CSS)
- Turnstile sandbox errors in iframes
- JavaScript errors when API returns HTML instead of JSON

---

## 📂 Repository Contents

| Folder | Contents |
|--------|----------|
| `DOXBIN_INTELLIGENCE.md` | Complete raw intelligence dossier |
| `NATIONAL_SECURITY_ALERT.md` | Legal warning and threat assessment |
| `reports/` | Archived intelligence reports |
| `logs/` | Redacted logs |

All data is **redacted** to protect sensitive information.

---

## 🚨 Call to Action

1. **Hosting Providers** – Terminate service to AS211273 and its subsidiaries
2. **Cloudflare** – Reevaluate allowing Doxbin to use their services
3. **Law Enforcement** – Coordinate cross-jurisdictional takedown
4. **Policymakers** – Strengthen laws against doxing and cyber-harassment
5. **Citizens & Journalists** – Share this dossier and raise awareness

---

## 🤝 How You Can Help

- **Star & Fork** this repository to increase visibility
- **Contribute** evidence, court documents, or additional intelligence
- **Contact** your representatives and demand action
- **Report** illegal content to appropriate authorities

---

## ⚖️ Disclaimer

This repository is intended for **educational, research, and awareness purposes** only. All information is derived from publicly available sources and has been redacted to protect privacy. The authors do not condone or encourage any illegal activity.

---

## ❄️ WHAT A FREEZE

*Documentation. Awareness. Accountability.*

– **WinterGate IC Command**

---

**Repository:** [https://github.com/WinterGate-IC/doxbin-excision](https://github.com/WinterGate-IC/doxbin-excision)
