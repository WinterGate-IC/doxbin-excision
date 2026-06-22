# Operation Freeze – Exposing Cloud Software - FZCO (AS211273)
## A Criminal Hosting Empire Enabling Doxing, Cybercrime, and National Security Threats

**Classification:** PUBLIC INTELLIGENCE DOSSIER  
**Date:** 2026-06-22  
**Source:** WinterGate Intelligence Collective (WIC)  
**Status:** VERIFIED – ACTIVE INFRASTRUCTURE  

---

## 1. Executive Summary

**Cloud Software - FZCO (AS211273)** – a Dubai‑based holding company – is operating a criminal hosting empire that knowingly and intentionally enables some of the most dangerous activities on the internet.

This dossier exposes:

- **The full corporate structure** – HostVDS, Cloudzy, Doxbin, Subnet Digital, RouterHosting LLC.
- **Physical infrastructure** – data centers in Dubai, Latvia, France, and the United States.
- **The U.S. presence** – EGIHosting (Santa Clara, CA) providing physical infrastructure for hostile activity.
- **National security threats** – hosting Iranian state‑sponsored APT groups (APT34, MuddyWater, APT35, APT33).
- **Permanent blacklisting** – IP ranges with 100% confidence abuse reports.
- **A pattern of criminal liability** – knowingly hosting doxing, swatting, harassment, and cybercrime platforms.

**This is not a failure of security. This is a deliberate business model.**

---

## 2. The Corporate Structure

Cloud Software - FZCO (AS211273) operates through a network of subsidiary brands designed to obscure ownership and evade accountability.

| Entity | Role | Location | IP Ranges |
|--------|------|----------|-----------|
| **Cloud Software - FZCO** | Parent Holding Company | Dubai, UAE | AS211273 |
| **HostVDS** | Infrastructure Provider | Latvia / UAE | `45.38.0.0/16`, `45.39.0.0/16` |
| **Cloudzy** | VPS / Cloud Provider | UAE / USA | `144.172.0.0/16`, `107.189.0.0/16` |
| **Doxbin** | Criminal Doxing Platform | Germany | `185.53.179.200`, `185.53.179.145` |
| **Subnet Digital** | Subsidiary Host | Finland / USA | `45.38.198.0/24` |
| **RouterHosting LLC** | Subsidiary Host | USA | `172.86.0.0/16` |

**Key Finding:** All entities share infrastructure, upstream providers, and physical data centers. They are not separate companies – they are a single hostile network operating under a shell corporate structure.

---

## 3. Physical Infrastructure (Confirmed)

| Location | Address | Provider | IP Ranges Hosted |
|----------|---------|----------|------------------|
| **Santa Clara, CA, USA** | 3223 Kenneth Street, Santa Clara, CA 95054 | EGIHosting (AS18779) | `45.38.0.0/16`, `45.39.0.0/16` |
| **Paris, France** | Cloud Software - FZCO Data Center | Cloud Software - FZCO | `95.182.81.0/24` |
| **Riga, Latvia** | Cloud Software - FZCO Data Center | Cloud Software - FZCO | `104.253.25.0/24` |
| **Dubai, UAE** | Cloud Software - FZCO Headquarters | Cloud Software - FZCO | `95.182.89.0/24` |

**U.S. Presence Confirmed:**  
AS211273 infrastructure is physically hosted by EGIHosting (AS18779) at 3223 Kenneth Street, Santa Clara, CA. This gives the United States full legal jurisdiction over the hostile infrastructure.

**Federal Awareness:**  
Recent intelligence bulletins and National Security Agency (NSA) advisories have flagged AS211273 for hosting Iranian state‑sponsored cyber actors (APT34, MuddyWater, APT35, APT33). U.S. critical infrastructure operators have been advised to block all traffic from these IP ranges.

---

## 4. Knowingly Hosting Malicious Entities

Cloud Software - FZCO does not merely "fail" to moderate content – they actively provide infrastructure to known malicious actors.

### 4.1 Doxbin
- Doxbin is a platform dedicated to doxing, swatting, harassment, and identity theft.
- It has been used to expose the personal data of hundreds of Spanish judges, prosecutors, and National Police officers.
- The platform was seized once by law enforcement and reappeared under the same ASN.

### 4.2 Iranian State‑Sponsored APTs
AS211273 has been confirmed as hosting:
- **APT34 (OilRig)** – Iranian cyber‑espionage group targeting government and critical infrastructure.
- **MuddyWater** – Iranian APT conducting intelligence gathering and supply chain attacks.
- **APT35 (Charming Kitten)** – Iranian APT conducting credential theft and espionage.
- **APT33 (Elfin)** – Iranian APT targeting the energy and aviation sectors.

**National Security Advisory:**  
U.S. critical infrastructure operators have been advised to block all traffic from AS211273 IP ranges. Cloud Software - FZCO is knowingly providing infrastructure to threat actors targeting the United States.

### 4.3 Cybercrime and Fraud
- HostVDS and Cloudzy are used by cybercriminals to host phishing domains, malware C2 servers, and credential‑stealing platforms.
- The ASN has a **0.68% spam rate** (CleanTalk) – permanently damaging the reputation of all IPs.
- Abuse reports confirm SSH brute‑force, web attacks, SQL injection, and credential harvesting originating directly from AS211273 IPs.

---

## 5. Permanent Blacklisting (100% Confidence)

The following IPs are permanently flagged with 100% confidence on AbuseIPDB. Any new IP added to AS211273 inherits this reputation damage.

| IP Address | Location | Reports | Sources | Activity |
|------------|----------|---------|---------|----------|
| `95.182.81.25` | Paris, FR | **1,453** | **938** | SSH brute‑force, web attacks, SQL injection |
| `95.182.89.109` | Kansas City, US | **148** | **120** | `.env` scanning, web attacks |
| `104.253.25.218` | Riga, LV | **212** | **160** | SSH brute‑force with username list: `ibrahim`, `steam`, `root`, `noc`, `carlos`, `chris`, `koha`, `deploy` |
| `45.39.84.135` | Riga, LV | **Blacklisted** | **N/A** | WordPress spam on 96+ sites |

**Spamhaus, Barracuda, AbuseIPDB, CleanTalk, UCEPROTECT** – all have permanently flagged AS211273 IP ranges.

---

## 6. Exposed Vulnerabilities (Weaponized)

The infrastructure itself is vulnerable, and these vulnerabilities were successfully exploited.

| Vulnerability | Endpoint | Status | Evidence |
|---------------|----------|--------|----------|
| **Reflected XSS** | `/search?q=...` | Confirmed | Payload `<h1>FROZEN</h1>` echoed unescaped. |
| **Stored XSS** | `/api/user/comment/create` | Confirmed | Worm injected into staff profiles. |
| **SQL Injection** | `/search?q=...` | Confirmed | Error messages reveal MySQL syntax errors. |
| **State Table Exhaustion** | Upstream Gateway | Confirmed | tcpdump logs show packets silently dropped after authentication. |
| **API Misconfiguration** | `/api/index/pastes` | Confirmed | Returns HTML error pages instead of JSON. |

**CERT/CC Reference:** The state table exhaustion attack vector is documented in CERT/CC advisory VU#539363 (CVE-2002-2150) – a 23‑year‑old vulnerability that was left unpatched.

---

## 7. National Security Threat

Cloud Software - FZCO is not a "neutral" hosting provider. They are a **critical enabler** of threats to national security.

- **Iranian Cyber Operations:** AS211273 hosts APT groups that target U.S. critical infrastructure.
- **Doxing of Government Personnel:** Doxbin has published personal data of judges, prosecutors, and law enforcement officers.
- **Swatting and Harassment:** Doxbin has been linked to swatting attacks against U.S. citizens and government officials.
- **Financial Fraud:** The network enables credential theft, identity theft, and financial fraud.

**Federal Action Required:**  
The U.S. government must treat Cloud Software - FZCO and its subsidiaries as a **hostile entity** operating on U.S. soil. This includes:

- Economic sanctions.
- Seizure of assets.
- Criminal prosecution of executives.
- Termination of EGIHosting's ability to host AS211273 infrastructure.

---

## 8. Legal Exposure

| Entity | Jurisdiction | Violation |
|--------|--------------|-----------|
| Cloud Software - FZCO | UAE / France / Latvia / USA | Hosting cybercrime, doxing, harassment, and APT infrastructure |
| EGIHosting (AS18779) | Santa Clara, CA, USA | Providing physical infrastructure to a hostile network |
| HostVDS / Cloudzy | Latvia / UAE | Enabling cybercrime and fraud |
| Doxbin | Germany / USA | Doxing, swatting, harassment, identity theft |

**Civil Liability:**  
Victims of Doxbin's doxing campaigns have standing to sue Cloud Software - FZCO, EGIHosting, and all subsidiary entities for damages.

**Criminal Liability:**  
Violation of U.S. federal law:
- 18 U.S.C. § 1030 (Computer Fraud and Abuse Act)
- 18 U.S.C. § 2261A (Cyberstalking)
- 18 U.S.C. § 875 (Interstate Communications)
- 18 U.S.C. § 371 (Conspiracy)

---

## 9. Recommendations

### For Law Enforcement:
- Coordinate with INTERPOL, Europol, and the FBI.
- Serve subpoenas to EGIHosting (AS18779) for records of AS211273 infrastructure.
- Seize assets of Cloud Software - FZCO and its subsidiaries.
- Indict executives for criminal conspiracy.

### For Hosting Providers:
- Terminate service to AS211273 immediately.
- Block all IP ranges associated with Cloud Software - FZCO.
- Report violations to the FBI's Cyber Division.

### For Security Teams:
- Block all traffic from IP ranges listed above.
- Monitor for known attack patterns from AS211273.
- Report any AS211273 activity to law enforcement.

### For the Public:
- Report any Doxbin‑related activity to local law enforcement.
- Share this dossier to raise awareness.

---

## 10. Conclusion

Cloud Software - FZCO (AS211273) is a **criminal hosting empire** that knowingly enables doxing, swatting, harassment, identity theft, and Iranian cyber‑espionage. This is not a failure of security – it is a deliberate business model.

The infrastructure is permanently exposed, globally blacklisted, and vulnerable to exploitation.

**Federal attention is required. National security is at risk.**

**❄️ WHAT A FREEZE**

– WinterGate Intelligence Collective (WIC) –
