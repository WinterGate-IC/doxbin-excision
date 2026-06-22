# Operation: Excision – Exposing the Doxbin Criminal Platform

*An open‑source intelligence dossier for national security, law enforcement, and public awareness.*

---

## 📌 Overview

**Doxbin** is a platform that has enabled doxing, swatting, identity theft, harassment, and violence coordination for over a decade. It has been used to target judges, prosecutors, military personnel, journalists, and private citizens worldwide. Despite occasional takedowns, it reappears under new ownership, facilitated by a fragmented network of malicious actors.

This repository is a **public intelligence resource** – documenting the criminality, infrastructure, and threat posed by Doxbin to national security.

---

## 🎯 Purpose

- **Document** Doxbin’s history, crimes, and patterns of abuse.
- **Expose** links to extremist groups, including the Insanity Security Team and Atomwaffen Division.
- **Map** its hosting infrastructure, Cloudflare protections, and evasion tactics.
- **Provide** actionable evidence for law enforcement, policymakers, and hosting providers.
- **Raise awareness** of the threat to democratic institutions and public safety.

**This is not an attack. This is accountability.**

---

## 🧠 Why Doxbin Matters

| Threat | Impact |
|--------|--------|
| **Doxing** | Personal info of judges, military, and civilians exposed. |
| **Swatting** | Life‑threatening police responses triggered by false reports. |
| **Identity Theft** | Financial fraud and long‑term harm to victims. |
| **Extremist Coordination** | Used by Atomwaffen Division and Insanity Security Team for operational security and targeting. |
| **National Security** | Exposure of government personnel undermines trust and safety. |

---

## 🏛️ Legal & Ethical Context

- Doxbin operates in a legal gray area, but its content **violates laws** in multiple jurisdictions (GDPR, doxing laws, harassment statutes).
- Law enforcement has **seized** the platform before, but it resurfaces due to fragmented ownership.
- This dossier is **not** an attack; it is a **call to action** for lawful intervention.
- It aligns with **national security directives** to disrupt enemy networks and protect democratic institutions.

---

## 🕵️ Technical Analysis – Infrastructure & Protections

### Hosting & Upstream Providers

- **Parent ASN:** `AS211273` – Cloud Software FZCO (Dubai, UAE)
- **Upstream Host:** EGIHosting (Santa Clara, CA, USA) – provides physical infrastructure.
- **Sub‑brands:** HostVDS, Cloudzy, RouterHosting LLC, Subnet Digital.
- **Geographic Footprint:** Latvia, UAE, Finland, USA – using multiple jurisdictions to evade takedowns.

### Security Protections

- **Cloudflare:** Used for DDoS mitigation, bot management, and challenge pages.
- **Turnstile:** Cloudflare’s CAPTCHA alternative is deployed on login and comment forms.
- **Content Security Policy (CSP):** Strict policies with `nonce` and `unsafe-eval` exceptions.
- **State‑Table Firewalls:** Protect against volumetric attacks but are susceptible to exhaustion under sustained pressure.

### Evasion Tactics

- **Multiple Brands:** HostVDS, Cloudzy, and others share upstream infrastructure, creating a fragmented footprint.
- **Jurisdictional Arbitrage:** Operation from UAE, Latvia, and Finland complicates law enforcement coordination.
- **Rapid Re‑branding:** Ownership shifts frequently, making permanent takedown difficult.

---

## 📂 Repository Contents

| Folder | Contents |
|--------|----------|
| `reports/` | Intelligence dossiers, attack maps, and timeline analysis. |
| `logs/` | Redacted logs showing infrastructure behavior and abuse patterns. |
| `scripts/` | (Optional) Tools for reconnaissance and analysis (not attack tools). |

All data is **redacted** to protect sensitive information and comply with responsible disclosure practices.

---

## 🔍 What We Found

- **Reflected XSS** on `/search` – confirmed and exploitable.
- **Stored XSS** via comment system – allows persistent defacement and session theft.
- **SQL Injection** on `/search` – enables data extraction and file system interaction.
- **State‑Table Exhaustion** – upstream firewalls vulnerable to SYN floods, causing denial of service.
- **API Misconfiguration** – `/api/index/pastes` returns HTML instead of JSON, indicating poor error handling.
- **Weak Session Management** – tokens persist without rotation, enabling session hijacking.

These findings are **not** exploits to be weaponized; they are **evidence** of systemic security failures that need to be addressed by the platform and its providers.

---

## 🚨 Call to Action

1. **Hosting Providers** – Terminate service to AS211273 and its subsidiaries.
2. **Cloudflare** – Reevaluate allowing Doxbin to use their services.
3. **Law Enforcement** – Collaborate across jurisdictions to permanently shut down the platform.
4. **Policymakers** – Strengthen laws against doxing and cyber‑harassment.
5. **Citizens & Journalists** – Share this dossier and raise public awareness.

---

## 🤝 How You Can Help

- **Star & Fork** this repository to increase visibility.
- **Contribute** evidence, court documents, or additional intelligence.
- **Contact** your representatives and demand action.
- **Report** illegal content to appropriate authorities.

---

## ⚖️ Disclaimer

This repository is intended for **educational, research, and awareness purposes** only. All information is derived from publicly available sources and has been redacted to protect privacy. The authors do not condone or encourage any illegal activity.

---

## ❄️ WHAT A FREEZE

*Documentation. Awareness. Accountability.*

– **WinterGate IC Command**

**Repository:** [https://github.com/WinterGate-IC/doxbin-excision](https://github.com/WinterGate-IC/doxbin-excision)
