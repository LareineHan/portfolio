# Legitimate Infrastructure Abuse — Auvik / Demio / Zoho

_Sanitized portfolio case study (ISS/ITS) — May 5 ~ 11, 2026_

## Context

Multiple phishing and scam campaigns were observed abusing legitimate SaaS infrastructure to bypass normal email security controls.

The campaigns leveraged trusted platforms including:

- Auvik
- Demio
- Zoho
- Amazon SES

Because the infrastructure itself was legitimate, traditional reputation-based detection became unreliable and created both:
- false negatives (malicious emails delivered)
- false positives (legitimate platform traffic flagged)

---

## Problem

Determine:
- Was the infrastructure itself malicious or being abused?
- What indicators separated legitimate platform usage from phishing campaigns?
- Could stable behavioral patterns be identified despite randomized sender/header values?
- How could remediation occur without blocking entire trusted platforms?

---

## Constraints

- Shared SaaS infrastructure generated both legitimate and malicious traffic
- Sender/header identifiers rotated frequently
- Authentication often fully passed (SPF/DKIM/DMARC)
- Trusted infrastructure reputation reduced detection effectiveness
- Full platform blocking would create unacceptable operational impact

---

## Actions Taken

### 1) Auvik infrastructure abuse investigation

Investigated phishing emails using:
- `noreply@auvik.com`
- legitimate Auvik invitation infrastructure

Observed:
- sextortion / Bitcoin payment demands
- customized invitation links per recipient
- rotating tenant identifiers

Pattern observed:

```text
https://<tenant>.us6.my.auvik.com/invitation/accept/<recipient-id>
```

Identified recurring tenant identifiers:
- `bigeyes`
- `bigbro`
- `mevvhq`
- `bigeyeshq`
- `ikazmohq`

Additional observations:
- legitimate Auvik traffic usually used:
  - `envelope.auvik.com`
- phishing traffic instead routed through:
  - Amazon SES infrastructure

This suggested legitimate platform abuse rather than direct infrastructure compromise.

---

### 2) Demio webinar abuse investigation

Investigated phishing/scam emails delivered through:
- `notifications@demio.com`

The challenge:
- not all Demio traffic was malicious
- sender infrastructure was heavily shared
- sender/header numbers randomized constantly

Comparison between malicious and legitimate webinar traffic revealed a strong repeated indicator:

Malicious campaigns consistently embedded:
```text
cdn.demio.com/demio-logo-new.png
```

Legitimate webinar emails usually:
- used the hosting organization's logo
- did not prominently reuse the Demio platform logo itself

Additional pattern:
- many malicious subject lines exceeded 100 characters

Using these combined indicators:
- multiple malicious emails delivered into inboxes were identified
- linked landing pages redirected to malicious/scam destinations

---

### 3) Zoho / SES / Shared infrastructure validation

Additional investigations showed similar abuse patterns across:
- Zoho mail infrastructure
- Amazon SES
- shared webinar/email marketing services

This reinforced a broader operational challenge:

Trusted SaaS reputation alone cannot determine legitimacy.

Behavioral patterns and delivery context became more reliable than:
- sender reputation
- authentication pass status
- platform trust level

---

### 4) Vendor escalation & remediation

Prepared and submitted abuse reports to:
- Demio Security Team
- Auvik Security Team

Reports included:
- infrastructure abuse details
- malicious invitation workflows
- Bitcoin wallet indicators
- screenshots
- phishing samples
- delivery observations

This shifted remediation beyond internal blocking into:
- external infrastructure coordination
- vendor-side abuse response

---

## Evidence (Sanitized)

### Auvik
- Legitimate invitation infrastructure abused for phishing
- Rotating tenant IDs reused across campaigns
- SES-linked sender infrastructure
- Bitcoin extortion workflow

### Demio
- Shared platform infrastructure
- Demio-logo reuse pattern
- Extremely long scam-oriented subjects
- Inbox-delivered phishing traffic

### Shared Observations
- Authentication often fully passed
- Infrastructure reputation appeared legitimate
- Sender/header randomization reduced IOC usefulness

---

## Outcome

- Identified stable behavioral indicators inside shared trusted infrastructure
- Helped distinguish legitimate SaaS usage from malicious campaigns
- Supported targeted remediation without broad platform blocking
- Escalated abuse findings directly to affected vendors

---

## Impact

- Improved operational handling of trusted-platform abuse
- Reduced over-reliance on authentication verdicts alone
- Expanded investigations from IOC analysis → infrastructure behavior analysis
- Reinforced importance of platform-aware phishing detection strategies

---

## Lessons Learned

- Legitimate infrastructure can be one of the hardest phishing environments to investigate
- SPF/DKIM/DMARC pass does not imply legitimacy
- Shared SaaS infrastructure requires behavior-focused analysis
- Vendor escalation can become part of phishing operations workflows

---

## Next Steps

- Build reusable SaaS abuse detection heuristics
- Expand platform-behavior comparison workflows
- Improve operational playbooks for legitimate infrastructure abuse
- Continue developing behavioral indicators that minimize false positives

---

## KQL Used (Sanitized)

### Query A — Demio logo-based filtering

```kql
let FilteredUrls = EmailUrlInfo
| where Url contains "cdn.demio.com/demio-logo-new.png"
| project NetworkMessageId, Url;

EmailEvents
| join kind=inner FilteredUrls on NetworkMessageId
| project Timestamp, SenderFromAddress, Subject, LatestDeliveryLocation
| order by Timestamp desc
```

### Query B — Auvik invitation workflow review

```kql
EmailUrlInfo
| where Url contains "my.auvik.com/invitation/accept/"
| join kind=inner EmailEvents on NetworkMessageId
| project Timestamp, SenderFromAddress, Url, RecipientEmailAddress
| order by Timestamp desc
```

### Query C — Shared infrastructure validation

```kql
EmailEvents
| where SenderMailFromDomain contains "amazonses.com"
| summarize Count=count() by SenderFromAddress, LatestDeliveryLocation
| order by Count desc
```

---

## Sanitization Note

Infrastructure identifiers, tenant values, recipient information, URLs, and operationally sensitive indicators have been generalized or redacted.