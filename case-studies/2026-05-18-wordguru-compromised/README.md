# Fake Verification Scam → Sandbox Evasion → Compromised Website Investigation

_Sanitized portfolio case study (ISS/ITS) — May 18, 2026_

## Context

A recurring email campaign delivered suspicious links associated with `wordguru.io` infrastructure.

Although some emails were quarantined, many were still reaching inboxes with `ThreatType=None`.

Initial reputation scans showed inconsistent results, requiring deeper behavioral analysis to determine whether the links were truly malicious.

---

## Problem

Determine:
- Was the site directly malicious or a compromised legitimate website?
- Was the “verification” flow legitimate?
- Did the page attempt malware delivery or social engineering?
- Could sandbox behavior reveal hidden malicious activity?

---

## Constraints

- URL reputation tools produced inconsistent verdicts
- Some infrastructure appeared legitimate
- Sender behavior rotated frequently
- The visible landing page initially resembled a harmless quiz/trivia site
- Behavioral differences appeared depending on analysis environment

---

## Actions Taken

### 1) Initial landing-page review

The page initially presented itself as:
- a trivia/quiz page
- “human verification” workflow
- harmless educational-style interaction

However:
- sender volume was unusually high
- inbox delivery rates were concerning
- user interaction flow appeared suspicious

---

### 2) Sandbox investigation

Performed behavioral analysis using Windows Sandbox.

Observed:
- browser debugger pause behavior
- script execution changes when DevTools were open
- anti-analysis behavior
- different rendering depending on inspection state

This suggested sandbox/devtools-aware evasion behavior.

---

### 3) Social engineering workflow discovery

In a clean sandbox environment without DevTools open, the page revealed a fake “Verify you are human” prompt.

The page instructed users to:

1. Open Windows Run dialog (`Win + R`)
2. Launch PowerShell/Terminal
3. Paste a “verification code”
4. Execute commands manually

This behavior strongly matched:
- ClickFix-style social engineering
- user-assisted malware execution techniques

---

### 4) Infrastructure & network observations

Additional analysis revealed:
- Polygon Mainnet RPC calls
- unusual blockchain-related network activity
- unrelated infrastructure inconsistent with the visible “quiz” purpose

This suggested:
- reused scam infrastructure
- injected malicious scripts
- or compromised legitimate hosting

---

### 5) Infrastructure validation

Follow-up investigation suggested the site itself may have originally been legitimate but later compromised through:
- malicious script injection
- ad network abuse
- or embedded third-party payloads

The security team recommended:
- temporary remediation/soft-delete actions
- infrastructure owner notification
- monitoring for future campaigns

---

## Evidence (Sanitized)

- Fake CAPTCHA / “Verify you are human” workflow
- Win+R → PowerShell execution lure
- Sandbox-aware debugger behavior
- Polygon Mainnet RPC traffic
- Repeated inbox delivery despite suspicious behavior
- Rotating sender patterns associated with `wordguru.io`

---

## Outcome

- Confirmed the behavior was malicious social engineering rather than legitimate verification
- Distinguished compromised infrastructure from intentionally malicious hosting
- Helped support remediation decisions for recent delivered emails
- Improved internal understanding of sandbox-based behavioral validation

---

## Impact

- Expanded workflow from reputation-based analysis → behavior-based investigation
- Reinforced sandbox analysis as a primary investigative method
- Improved confidence in identifying social-engineering delivery chains
- Demonstrated the importance of behavioral analysis over reputation verdicts alone

---

## Lessons Learned

- URL reputation tools alone are insufficient for high-confidence validation
- Sandbox behavior often reveals hidden payload delivery logic
- Legitimate infrastructure can become malicious after compromise
- User-assisted malware execution remains an effective phishing technique

---

## Next Steps

- Expand sandbox investigation workflows
- Build repeatable detection logic for fake-verification scams
- Improve monitoring for PowerShell/social-engineering delivery patterns
- Continue correlating behavior-based indicators with telemetry data

---

## KQL Used (Sanitized)

### Query A — Sender infrastructure review

```kql
EmailEvents
| where SenderMailFromDomain contains "wordguru"
| summarize Count=count() by SenderFromAddress, LatestDeliveryLocation
| order by Count desc
```

### Query B — Inbox delivery validation

```kql
EmailEvents
| where LatestDeliveryLocation contains "Inbox"
| where SenderMailFromDomain contains "wordguru"
| project Timestamp, Subject, RecipientEmailAddress, LatestDeliveryLocation
| order by Timestamp desc
```

### Query C — Internal telemetry validation

```kql
DeviceNetworkEvents
| where RemoteUrl contains "wordguru"
| project Timestamp, DeviceName, InitiatingProcessFileName, RemoteUrl
| order by Timestamp desc
```

---

## Sanitization Note

Infrastructure indicators, URLs, sender identifiers, and internal telemetry details have been generalized or redacted for operational security purposes.