# Financial Spoofing Campaign → Mailgun Rotation → Detection Rule Tuning

_Sanitized portfolio case study (ISS/ITS) — May 8, 2026_

## Context

A recurring financial spoofing campaign was repeatedly reaching users through rotating sender domains and shared Mailgun infrastructure.

Although some emails were quarantined, many were still being delivered into inboxes with `ThreatType=None`, creating a persistent phishing exposure problem.

The campaign impersonated financial companies and attempted to collect sensitive user information through fake loan-related landing pages.

---

## Problem

Move beyond individual phishing email review and determine:

- Is this the same recurring campaign despite rotating domains?
- What indicators remain stable enough for detection?
- Which controls reduce malicious delivery without creating major false positives?
- What evidence is strong enough to justify blocking actions?

---

## Constraints

- Sender domains rotated constantly
- Mailgun infrastructure was shared with legitimate traffic
- IPs rotated frequently
- Mail flow rules could not directly filter on URL prefixes inside email bodies
- Some infrastructure overlap existed with legitimate senders

Escalation required stronger evidence than “the domains look suspicious.”

---

## Actions Taken

### 1) Financial spoofing pattern review

Observed repeated impersonation of legitimate financial companies using visually similar domains.

Examples included:

- `blueskyfinancial.com` → `bluesky-financial[.]com`
- `pinevalleyfinancial.org` → `pinevalley-financial[.]com`
- `westfin.com` → `westwood-financial[.]net`
- `lincolnfinancial.com` → `lincoln--financial[.]com`

The spoofed domains redirected users to fake financial landing pages intended to collect personal information.

An unusual observation:
multiple fake companies reused the same physical office address in email footers.

---

### 2) Shared infrastructure analysis (Mailgun)

Most senders followed patterns such as:

- `info@mg.[domain]`
- `info@gg.[domain]`

Examples included:

- `couponsnfc[.]com`
- `iiiol[.]com`
- `madexmerry[.]com`
- `phliqr[.]com`
- `vrivyaa[.]com`

The attackers rotated:
- sender domains
- Mailgun tokens
- sender formats

This suggested organized campaign infrastructure rather than isolated phishing attempts.

---

### 3) Campaign expansion using KQL pivots

Expanded the investigation from an initial MDO cluster of 253 emails into a larger infrastructure-level search.

Pivoted using:
- sender header patterns
- Mailgun infrastructure overlap
- URL behavior
- delivery outcomes

### Results

- Initial cluster: 253 emails
- Expanded scope: 1,505 related emails across 7 days
- Broader pattern search: 2,025 related emails
- Only 27 emails (6 senders) appeared legitimate after validation

---

### 4) Stable signature discovery

The strongest repeated indicator was the shared URL prefix:

`https://napp.`

Pattern:
`https://napp.[domain]/...`

Every reviewed email using this pattern belonged to the same malicious sender group.

This became the most reliable campaign signature compared to:
- sender domains
- sender IPs
- Mailgun tokens
- sender formatting

---

## Evidence (Sanitized)

- Shared malicious URL prefix: `https://napp.`
- Mailgun infrastructure overlap
- Rotating sender-domain behavior
- 1,505 related emails across 7 days
- 2,025 broader matches
- Only 27 legitimate emails identified
- Shared physical addresses reused across spoofed finance entities

---

## Outcome

Produced a campaign-level operational summary for the security team, including:

- infrastructure overlap analysis
- recurring sender patterns
- delivery outcomes
- false positive considerations
- strongest repeatable signature
- blocking recommendations

Based on the investigation, the security team implemented blocking actions for the identified malicious sender infrastructure.

---

## Impact

- Shifted workflow from isolated phishing review → campaign-level infrastructure analysis
- Improved detection confidence despite sender/domain rotation
- Helped support operational blocking decisions while minimizing false positives
- Reinforced the value of behavior-based detection over single IOC analysis

---

## Lessons Learned

- Shared infrastructure abuse requires stronger pivots than sender domains alone
- URL behavior patterns can outperform IP or domain indicators
- False positive evaluation is critical before recommending broad blocking
- Campaign-level detection often requires infrastructure correlation rather than content matching

---

## Next Steps

- Expand reusable detection logic for recurring spoofing campaigns
- Build safer transport-rule indicators using adjacent sender behavior
- Improve infrastructure-based clustering workflows
- Continue refining detection tuning methodologies

---

## KQL Used (Sanitized)

### Query A — Shared sender pattern expansion

```kql
EmailEvents
| where SenderFromAddress startswith "info@mg."
    or SenderFromAddress startswith "info@gg."
| summarize Count=count() by SenderFromAddress, SenderMailFromDomain
| order by Count desc
```

### Query B — URL signature pivot

```kql
let FilteredUrls = EmailUrlInfo
| where Url contains "://napp"
| project NetworkMessageId, Url;

EmailEvents
| join kind=inner FilteredUrls on NetworkMessageId
| project Timestamp, SenderFromAddress, RecipientEmailAddress, Url, LatestDeliveryLocation
| order by Timestamp desc
```

### Query C — False positive validation

```kql
EmailUrlInfo
| where Url contains "://napp."
| join kind=inner EmailEvents on NetworkMessageId
| summarize Count=dcount(NetworkMessageId) by SenderFromAddress
| order by Count desc
```

---

## Sanitization Note

Internal tenant identifiers, recipient information, raw URLs, and operationally sensitive details have been generalized or omitted.