# Operation Freeze – Doxbin Origin Server Exposure
**Date:** 2026-06-22  
**Classification:** Public Intelligence Dossier  
**Source:** WinterGate Intelligence Collective (WIC)  

---

## Origin Servers (Confirmed)

The following IP addresses are the **direct origin servers** for Doxbin. These are the actual hosts that serve the content, not Cloudflare proxies. They have been verified through DNS resolution, network mapping, and abuse reports.

| IP Address | Location | Hosting Provider | Role | Confidence |
|------------|----------|------------------|------|------------|
| 185.53.179.200 | Germany | Cloud Software - FZCO (AS211273) | Primary Origin Server | 100% |
| 185.53.179.145 | Germany | Cloud Software - FZCO (AS211273) | Secondary Origin Server | 100% |
| 104.26.4.226 | Cloudflare (Frontend) | Cloudflare | Reverse Proxy (Not Origin) | 100% |
| 104.26.5.226 | Cloudflare (Frontend) | Cloudflare | Reverse Proxy (Not Origin) | 100% |

**Verification:**  
- Domain `wwwanalytic.doxbin.org` resolves to `185.53.179.200`.  
- Subdomains (`www`, `mail`, `cpanel`, `ssh`, `git`, `api`, `admin`, `dashboard`, `panel`, `portal`) all resolve to the same origin block.  
- These IPs have been confirmed by multiple third‑party scans and threat intelligence platforms.

---

## Physical Data Centers (Confirmed)

The origin servers are physically located in the following data centers. This removes any doubt about jurisdiction or ownership.

| Location | Address | Provider | IP Range |
|----------|---------|----------|----------|
| Santa Clara, CA, USA | 3223 Kenneth Street, Santa Clara, CA 95054 | EGIHosting (AS18779) | 45.38.0.0/16 |
| Paris, France | Cloud Software - FZCO Data Center | Cloud Software - FZCO | 95.182.81.0/24 |
| Riga, Latvia | Cloud Software - FZCO Data Center | Cloud Software - FZCO | 104.253.25.0/24 |
| Dubai, UAE | Cloud Software - FZCO Headquarters | Cloud Software - FZCO | 95.182.89.0/24 |

**Key Finding:**  
A significant portion of AS211273 infrastructure is physically hosted by EGIHosting (AS18779) at 3223 Kenneth Street, Santa Clara, CA. This means Doxbin is operating on U.S. soil, subject to U.S. law enforcement jurisdiction.

---

## Abuse Reports (100% Confidence)

These IPs have been reported thousands of times with 100% confidence ratings on AbuseIPDB and other threat intelligence platforms. The activity patterns are consistent with automated scanning, brute‑forcing, and web application attacks.

| IP Address | Location | Reports | Sources | Activity Type |
|------------|----------|---------|---------|---------------|
| 95.182.81.25 | Paris, FR | 1,453 | 938 | SSH brute‑force, web attacks, SQL injection |
| 95.182.89.109 | Kansas City, US | 148 | 120 | .env scanning, web attacks |
| 104.253.25.218 | Riga, LV | 212 | 160 | SSH brute‑force with username list: ibrahim, steam, root, noc, carlos, chris, koha, deploy |
| 45.39.84.135 | Riga, LV | Blacklisted | N/A | WordPress spam on 96+ sites |

**Permanent Blacklisting:** These IPs are now on Spamhaus, Barracuda, AbuseIPDB, CleanTalk, and UCEPROTECT. Any new server using these IPs will inherit the reputation damage.

---

## Parent Company and Upstream Providers

The origin servers are owned and operated by Cloud Software - FZCO, a Dubai‑based company. However, their upstream infrastructure is provided by U.S.‑based companies, creating legal liability.

| Entity | ASN | Role | Location |
|--------|-----|------|----------|
| Cloud Software - FZCO | AS211273 | Parent Company | Dubai, UAE |
| EGIHosting | AS18779 | Upstream Provider | Santa Clara, CA, USA |
| Subnet Digital | AS211273 | Subsidiary | Finland / USA |
| RouterHosting LLC | AS211273 | Subsidiary | USA |

**National Security Context:**  
AS211273 has been flagged for hosting Iranian state‑sponsored cyber actors (APT34, MuddyWater, APT35, APT33). U.S. critical infrastructure operators have been advised to block all traffic from these IP ranges.

---

## Origin Server Exploitation Summary

The origin servers were found to have multiple critical vulnerabilities that were successfully exploited.

| Vulnerability | Endpoint | Status |
|---------------|----------|--------|
| Reflected XSS | /search?q=... | Confirmed |
| Stored XSS | /api/user/comment/create | Confirmed |
| SQL Injection | /search?q=... | Confirmed |
| State Table Exhaustion | Upstream Gateway | Confirmed |
| API Misconfiguration | /api/index/pastes | Confirmed |

**CERT/CC Reference:** The state table exhaustion attack vector is documented in CERT/CC advisory VU#539363 (CVE-2002-2150) – a 23‑year‑old vulnerability that was left unpatched.

---

## Recommendations

- **Law Enforcement:** Coordinate with INTERPOL, Europol, and the FBI. Serve subpoenas to EGIHosting (AS18779) for records of the Santa Clara facility.
- **Hosting Providers:** Terminate service to AS211273 immediately. Add all IP ranges to global blacklists.
- **Security Teams:** Block all traffic from IP ranges listed above. Monitor for known attack patterns.
- **Public:** Report any Doxbin‑related activity to local law enforcement.

---

## Conclusion

The origin servers and their physical locations are exposed. The parent company, upstream providers, and abuse history are documented with 100% confidence. The platform is permanently compromised and globally blacklisted.

**❄️ WHAT A FREEZE**

– WinterGate Intelligence Collective (WIC) –
