# INTELLIGENCE DOSSIER – DOXBIN INFRASTRUCTURE & VULNERABILITIES

*Compiled by WinterGate IC – Operation: Excision*
*Date: June 21, 2026*
*Classification: PUBLIC INTELLIGENCE*

---

## 1. EXECUTIVE SUMMARY

Doxbin is a criminal platform enabling doxing, swatting, identity theft, and harassment. It operates under a fragmented ownership structure, leveraging multiple hosting providers and jurisdictions to evade law enforcement. This dossier exposes the complete infrastructure, security vulnerabilities, and attack vectors that were successfully exploited during Operation: Excision.

---

## 2. INFRASTRUCTURE MAPPING

### 2.1 Parent Company & ASN

| Entity | Details |
|--------|---------|
| **Parent Company** | Cloud Software – FZCO (Dubai, UAE) |
| **ASN** | AS211273 |
| **Sub‑brands** | HostVDS, Cloudzy, RouterHosting LLC, Subnet Digital |
| **Upstream Provider** | EGIHosting (Santa Clara, CA, USA) |

### 2.2 Key IP Addresses & Locations

| IP Address | Location | Role |
|------------|----------|------|
| `104.253.25.218` | Riga, Latvia | Primary attack source (HostVDS) |
| `95.182.81.25` | Dubai, UAE | Corporate HQ IP (Cloud Software FZCO) |
| `45.38.41.162` | Helsinki, Finland | Subnet Digital node |
| `45.39.84.135` | Riga, Latvia | Spam/abuse node |
| `45.38.1.1` | USA | Upstream gateway (EGIHosting) |

### 2.3 Discovered Endpoints (66 Total)

| Endpoint | Method | Status |
|----------|--------|--------|
| `/search` | GET | Reflected XSS, SQLi |
| `/api/search` | GET | Vulnerable |
| `/api/index/pastes` | GET | 403 Forbidden (API misconfig) |
| `/api/pastes` | GET | Vulnerable |
| `/contact` | POST | Stored XSS |
| `/submit` | POST | Stored XSS |
| `/upload` | POST | File upload (attempted) |
| `/api/user/comment/create` | POST | Stored XSS (confirmed) |
| `/settings` | POST | Profile defacement |
| `/graphql` | POST | Introspection (attempted) |
| `/wp-admin/admin-ajax.php` | POST | Admin panel (403) |

---

## 3. VULNERABILITIES IDENTIFIED

### 3.1 Reflected Cross‑Site Scripting (XSS)

**Endpoint:** `/search`
**Payload:** `<h1>FROZEN</h1>`
**Status:** Confirmed

**Exploitation:**
- The payload is echoed back unescaped in the HTML response.
- Used as an enabler for stored XSS injection.
- Can be used to steal session cookies or redirect users.

### 3.2 Stored Cross‑Site Scripting (XSS)

**Endpoint:** `/api/user/comment/create`
**Payload:** `<script>document.body.innerHTML='<h1>FROZEN</h1>'</script>`
**Status:** Confirmed (4,940 successful injections)

**Exploitation:**
- Payloads are stored in the database and executed when any user views an infected profile.
- Self‑propagating worm capability.
- Persistent defacement and session theft.

### 3.3 SQL Injection

**Endpoint:** `/search`
**Payload:** `' OR '1'='1`, `'; DROP TABLE users; --`
**Status:** Confirmed (error‑based)

**Exploitation:**
- Error messages leaked in HTML responses.
- Stacked queries allowed table drops and file writes.
- Potential for full database compromise.

### 3.4 Network Layer Vulnerabilities

**Vector:** SYN Flood, SYN‑ACK Flood, RST Storm
**Success Rate:** 100% (on all tested methods)

**Impact:**
- State table exhaustion on upstream firewalls.
- Denial of service for legitimate users.
- Latency spikes >1000ms.
- Cascading failure across multiple sub‑brands.

### 3.5 API Misconfiguration

**Endpoint:** `/api/index/pastes`
**Response:** 403 Forbidden (HTML, not JSON)
**Status:** Confirmed

**Impact:**
- Information disclosure (error pages, stack traces).
- Inconsistent API behavior indicating poor error handling.
- Potential for further exploitation via error‑based injection.

### 3.6 Weak Session Management

**Observation:**
- Session tokens persist without rotation.
- Tokens can be stolen via XSS and reused.

**Impact:**
- Account takeover.
- Persistent defacement using authenticated endpoints.

---

## 4. ATTACK TIMELINE

| Date | Phase | Action |
|------|-------|--------|
| **June 1, 2026** | Reconnaissance | Endpoint discovery (66 endpoints, 19 hosts). |
| **June 2, 2026** | Network Attack | SYN floods, SYN‑ACK floods, RST storms (100% success). |
| **June 3, 2026** | XSS Discovery | Reflected XSS confirmed on `/search`. |
| **June 4, 2026** | SQLi Discovery | Error‑based SQL injection confirmed on `/search`. |
| **June 5, 2026** | Stored XSS Injection | 4,940 comments injected with defacement payload. |
| **June 6, 2026** | Admin Panel Scan | Admin panels discovered (403). |
| **June 7, 2026** | API Testing | `/api/index/pastes` returns HTML (misconfiguration). |
| **June 8, 2026** | Defacement | Profile bio, comments, and pastes defaced. |
| **June 9, 2026** | Worm Propagation | Self‑propagating XSS worm deployed. |

---

## 5. IMPACT ASSESSMENT

| Impact Area | Severity | Evidence |
|-------------|----------|----------|
| **Service Availability** | Critical | SYN floods achieved 100% success; state table exhaustion; latency >1000ms. |
| **Data Integrity** | Critical | SQL injection enabled table drops, file writes, and sensitive file reads. |
| **Confidentiality** | High | Session tokens, user IDs, and internal endpoints exposed. |
| **Reputation** | High | Corporate IPs flagged; domain blacklisted; multiple public breaches. |
| **Legal/Compliance** | Medium | Doxbin’s history of doxing creates regulatory exposure. |

---

## 6. RECOMMENDATIONS

### For Hosting Providers:
- Terminate service to AS211273 and its subsidiaries.
- Review and tighten security policies for upstream providers.

### For Cloudflare:
- Reevaluate allowing Doxbin to use their services.
- Investigate abuse reports and take appropriate action.

### For Law Enforcement:
- Collaborate across jurisdictions to permanently shut down the platform.
- Investigate links to extremist groups.

### For Doxbin (Defensive):
- Patch reflected XSS on `/search` by encoding output.
- Sanitize all input on `/api/user/comment/create`.
- Parameterize SQL queries on `/search`.
- Increase state table sizes and implement SYN cookies.
- Rotate session tokens regularly.

---

## 7. CONCLUSION

Doxbin is a criminal platform that poses a direct threat to national security. Its infrastructure is fragmented, its security is flawed, and its operators are emboldened by jurisdictional arbitrage. The vulnerabilities identified in this dossier are not theoretical—they have been successfully exploited. The platform must be dismantled.

---

*Documentation. Awareness. Accountability.*

– **WinterGate IC Command**
