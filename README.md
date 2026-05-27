## 🛡️ Cybersecurity Portfolio

Lareine Han  
Security Operations | Phishing Infrastructure Analysis | Redirect & Evasion Investigation | False Positive Validation



---

# 👋 About This Repository

This repository contains sanitized cybersecurity investigations and operational case studies from my work in Security Operations.

My work focuses heavily on phishing infrastructure analysis, redirect-chain investigation, false positive validation, and detection tuning using Microsoft Defender XDR, KQL, Entra ID telemetry, and sandbox-based behavioral analysis.

Many investigations involve distinguishing malicious infrastructure abuse from legitimate SaaS or marketing behavior, especially in cases where attackers hide behind layered redirects, shared cloud infrastructure, or trusted platforms.

This includes:

- false positive validation and release decisions
- quarantine and sender-domain blocking recommendations
- shared platform abuse investigations (AWS SES, Zoho, Demio, Auvik, Mailgun)
- vendor escalation when legitimate infrastructure is being abused
- detection tuning to reduce both missed attacks and false positives
- sandbox-based behavioral investigation
- infrastructure correlation and campaign scoping

All content is anonymized and redacted to protect sensitive internal information.



---

# 📂 Case Studies


## 🔎 1. Redirect-Based Crypto Impersonation Campaign Analysis
- Investigated a GRASS token impersonation/scam campaign using layered redirect infrastructure
- Identified ClickFunnels + SendGrid tracking abuse with rotating `.za.com` downstream domains
- Discovered conditional redirect behavior depending on whether links were opened directly from the rendered email flow
- Compared rendered Google Workspace email behavior against Microsoft Defender URL observations
- Used sandbox browser + DevTools Network tracing to reconstruct redirect chains
- Observed inactive/404 fallback behavior during manual analysis attempts
- Identified infrastructure patterns suggesting anti-analysis / scanner-evasion logic
- Distinguished malicious impersonation behavior from legitimate GRASS notification infrastructure
- Created KQL hunts using redirect infrastructure indicators and tracking identifiers
- Correlated sender rotation and infrastructure reuse across related campaigns
- Demonstrated behavioral phishing analysis beyond static IOC or URL reputation checks
- 
➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-05-26-crypto-redirect-nalysis)

## 🔎 2. AWS SES + Mandrill Campaign-Level Pivot

- DMARC BestGuessPass via Amazon SES infrastructure
- Tracking-ID pivot for campaign scoping
- KQL joins across EmailEvents, EmailUrlInfo, UrlClickEvents
- 104-message scope validation
- Click telemetry impact assessment

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-02-24-aws-ses-mandrill)



## 🔎 3. Financial Spoofing Campaign → Mailgun Rotation → URL Signature Detection

- Expanded from a single MDO cluster (253 emails) into 1,500+ related messages
- Identified recurring sender rotation using Mailgun infrastructure and spoofed finance domains
- Detected near-100% malicious match using shared URL signature
- Evaluated IP filtering vs sender blocking vs URL-based indicators
- Proposed safer mitigation balancing detection strength and false positive risk
- Recommended sender/domain blocking based on infrastructure overlap

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-05-08-financial-spoofing)



## 🔎 4. Fake Verification Scam → Sandbox Evasion → Compromised Website Investigation

- Investigated malicious “Verify you are human” social engineering workflow
- Identified sandbox-aware evasion behavior
- Compared browser behavior with and without developer tools
- Investigated suspicious Polygon Mainnet RPC traffic
- Correlated behavioral indicators across sandbox telemetry and network artifacts
- Distinguished between compromised legitimate infrastructure vs fully malicious hosting

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-05-18-wordguru-compromised)



## 🔎 5. Japanese Spear Phishing → ASN 29873 Infrastructure Pivot → Account Containment

- Multi-stage redirect with bot-evasion behavior (HTTP 429 response)
- Infrastructure clustering within ASN 29873
- Header-level relay artifact pivot (eigbox.net)
- Identity correlation and authentication anomaly review
- 3 user password resets as protective action

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-03-03-asn29873)



## 🔎 6. Legitimate Infrastructure Abuse — Auvik / Demio / Zoho

- Investigated legitimate SaaS infrastructure abused for phishing delivery
- Distinguished malicious campaigns from valid webinar and IT automation traffic
- Analyzed sender rotation and infrastructure reuse patterns
- Identified detection gaps caused by trusted SaaS infrastructure

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-05-11-legit-infra-abused)



## 🔎 7. False Positive Validation & Detection Tuning

Examples include:

- Microsoft Store campaigns misclassified by fingerprint detection
- Canvas/Instructure notification false positives
- Political fundraising emails incorrectly quarantined as phishing
- Educational digest platforms flagged by URL reputation engines
- Legitimate Microsoft CDN JavaScript assets flagged as malware signatures

This work focuses on balancing security enforcement with operational continuity and reducing alert fatigue caused by overly aggressive detections.

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-spring-detection-tuning)



---

# 🧪 False Positive & Detection Tuning

In addition to phishing investigations, I regularly validate and release legitimate emails incorrectly classified by automated detection systems.

This work often requires:

- infrastructure validation
- sender reputation analysis
- authentication review (SPF/DKIM/DMARC)
- URL behavior analysis
- sandbox verification
- delivery telemetry correlation
- detection root-cause investigation

The goal is not only to identify malicious activity, but also to prevent unnecessary disruption to legitimate communication workflows.



---

# 🧠 Investigation Themes

Across cases, I focus on:

- infrastructure-level pivots (ASN, hosting clusters)
- header artifact identification
- redirect behavior & evasion detection
- identity telemetry correlation
- evidence-based escalation vs monitor-only decisions
- KQL-based hunting and validation workflows
- behavioral analysis using sandbox environments
- distinguishing compromised infrastructure from intentionally malicious infrastructure
- redirect-chain reconstruction
- layered tracking infrastructure analysis
- behavioral discrepancies between rendered email flow and security tooling
- conditional redirect / anti-analysis behavior


---

# 🛠 Technical Focus Areas

- Microsoft Defender XDR Advanced Hunting
- Entra ID sign-in analysis
- Email authentication (SPF, DKIM, DMARC)
- Shared hosting abuse patterns
- URL redirect chain analysis
- IOC clustering methodology
- Sandbox-based behavioral analysis
- False positive validation workflows
- Detection tuning & remediation recommendations
- Shared SaaS/platform abuse investigations



---

# 🎯 Investigation Philosophy

I avoid escalating on a single “weird” indicator.

I look for layered corroboration across:

- email content & headers
- infrastructure artifacts
- hosting patterns
- identity telemetry
- delivery behavior
- sandbox observations

Security decisions should be supported by multiple aligned signals, not intuition alone.

Good security operations require not only detecting malicious activity, but also correctly identifying legitimate activity that should not be disrupted.

Modern phishing analysis often requires understanding behavior and infrastructure relationships — not simply identifying a single malicious URL.

---

# 📌 Sanitization Notice

All case studies are redacted and sanitized.

- no internal tenant identifiers
- no raw sensitive logs
- no personally identifiable user data
- no confidential organizational details

These write-ups are for educational and portfolio purposes only.



---

# ⚖️ Usage & Rights

All rights reserved.

This content may not be redistributed, modified, or reused without permission.



---

# 📫 Contact

If you would like to discuss any case study or technical approach:

[LinkedIn](www.linkedin.com/in/lareinehan/)  
|  
[GitHub](github.com/LareineHan/portfolio)
