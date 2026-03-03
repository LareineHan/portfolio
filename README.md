## 🛡️ Cybersecurity Portfolio

Lareine Han
Information Security Student Analyst (ISS/ITS)
Microsoft Defender | Entra ID | KQL | Phishing & Infrastructure Analysis



## 👋 About This Repository

This repository contains sanitized cybersecurity case studies and work samples from my experience as an Information Security Student Analyst.

Each case study documents:
	•	Initial detection
	•	Investigation methodology
	•	Technical pivots (email, infrastructure, identity)
	•	Evidence-based escalation decisions
	•	Lessons learned

All content is anonymized and redacted to protect sensitive internal information.



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
