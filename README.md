## 🛡️ Cybersecurity Portfolio

Lareine Han
Security Operations | Phishing Investigation | Detection Tuning | Microsoft Defender | KQL | Infrastructure Analysis



## 👋 About This Repository

This repository contains sanitized cybersecurity case studies and operational investigations from my work as an Information Security Student Analyst in Security Operations.

My work focuses on phishing detection, false positive validation, shared infrastructure abuse analysis, and evidence-based remediation decisions using Microsoft Defender, Entra ID, and KQL.

Rather than treating alerts as isolated events, I investigate campaign patterns across sender infrastructure, authentication behavior, URL delivery paths, and identity signals to determine the safest and most effective response.

This includes:

• false positive validation and release decisions
• quarantine and sender-domain blocking recommendations
• shared platform abuse investigations (AWS SES, Zoho, Demio, Auvik, Mailgun)
• vendor escalation when legitimate infrastructure is being abused
• detection tuning to reduce both missed attacks and false positives

All content is anonymized and redacted to protect sensitive internal information.

⸻



## 📂 Case Studies

🔎 1. Japanese Spear Phishing → ASN 29873 Infrastructure Pivot → Account Containment
	•	Multi-stage redirect with bot-evasion behavior (HTTP 429 response)
	•	Infrastructure clustering within ASN 29873
	•	Header-level relay artifact pivot (eigbox.net)
	•	Identity correlation and authentication anomaly review
	•	3 user password resets as protective action

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/tree/main/case-studies/2026-02-24-aws-ses-mandrill)



🔎 2. AWS SES + Mandrill Campaign-Level Pivot
	•	DMARC BestGuessPass via Amazon SES infrastructure
	•	Tracking-ID pivot for campaign scoping
	•	KQL joins across EmailEvents, EmailUrlInfo, UrlClickEvents
	•	104-message scope validation
	•	Click telemetry impact assessment

➡️ [Read Full Case Study](https://github.com/LareineHan/portfolio/blob/main/case-studies/2026-03-03-asn29873/README.md)￼



🔎 3. Financial Spoofing Campaign → Mailgun Rotation → URL Signature Detection
	• Expanded from a single MDO cluster (253 emails) into 1,500+ related messages
	• Identified recurring sender rotation using Mailgun infrastructure and spoofed finance domains
	• Detected near-100% malicious match using shared URL signature (https://napp.)
	• Evaluated IP filtering vs sender blocking vs URL-based indicators
	• Proposed safer mitigation balancing detection strength and false positive risk
	
➡️ Read Full Case Study - is coming..

🔎 4. Legitimate Infrastructure Abuse — Auvik / Demio / Zoho

➡️ Read Full Case Study - is coming..

## 🧠 Investigation Themes

Across cases, I focus on:
	•	Infrastructure-level pivots (ASN, hosting clusters)
	•	Header artifact identification
	•	Redirect behavior & evasion detection
	•	Identity telemetry correlation (multi-geo, failure/success patterns)
	•	Evidence-based escalation vs monitor-only decisions
	•	KQL-based hunting and validation workflows



## 🛠 Technical Focus Areas
	•	Microsoft Defender Advanced Hunting
	•	Entra ID sign-in analysis
	•	Email authentication (SPF, DKIM, DMARC)
	•	Shared hosting abuse patterns
	•	URL redirect chain analysis
	•	IOC clustering methodology



## 🎯 Investigation Philosophy

I avoid escalating on a single “weird” indicator.
I look for layered corroboration across:
	•	Email content & headers
	•	Infrastructure artifacts
	•	Hosting patterns
	•	Identity telemetry

Security decisions should be supported by multiple aligned signals, not intuition alone.



## 📌 Sanitization Notice

All case studies are redacted and sanitized.
	•	No internal tenant identifiers
	•	No raw sensitive logs
	•	No personally identifiable user data
	•	No confidential organizational details

These write-ups are for educational and portfolio purposes only.



## ⚖️ Usage & Rights

All rights reserved.

This content may not be redistributed, modified, or reused without permission.



## 📫 Contact

If you would like to discuss any case study or technical approach:


[LinkedIn](www.linkedin.com/in/lareinehan/)
| 
[GitHub](github.com/LareineHan/portfolio)
