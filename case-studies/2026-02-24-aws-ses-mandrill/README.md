# AWS SES + Mandrill Pivot: From One Phish Report to Campaign-Level Scoping (104 emails)

_Sanitized portfolio case study (ISS/ITS) — Feb 24, 2026_

## Context
A user reported a **Robinhood-themed phishing email**. At first glance, the message looked “cleaner” than typical phish because authentication signals were not an obvious fail.

## Problem
Move beyond single-email triage and answer:
- Is this an isolated message or a distributed campaign?
- Did any variants reach Inbox (false negatives)?
- Did anyone click (impact)?
- What stable pivot can link variants even when subjects/senders rotate?

## Constraints
- Major platforms (AWS SES / marketing infrastructure) can make messages look reputable.
- URL reputation tools can lag behind brand-new domains.
- Escalation needs evidence: delivery outcomes + click telemetry, not “looks suspicious.”

## Actions Taken
### 1) Header + infrastructure review (why it “looked legit”)
- Observed **DMARC result = BestGuessPass** on the message.
- Identified the message route as **Amazon SES** (`...amazonses.com...` patterns in message identifiers/headers).
- Working hypothesis: SES infrastructure reputation can reduce friction for newly created or suspicious sender domains.

### 2) Find a stable campaign fingerprint (Mandrill tracking)
- The URL chain included `mandrillapp.com/track/click/...`.
- Extracted a stable identifier:
  - **Mandrill tracking/account ID: `31541291`**
- Used that ID as a pivot (more durable than subject keywords).

### 3) Campaign scoping with Defender telemetry + KQL pivots
- Pivoted across:
  - `EmailUrlInfo` (URLs in email)
  - `EmailEvents` (delivery outcomes)
- Scoped to messages tied to the same Mandrill ID and SES-linked deliveries.

**Result (scope)**
- **104 related emails** identified on **Feb 24** (campaign-level visibility).
- Mixed delivery outcomes observed (some filtered, some delivered to Inbox).

### 4) Impact validation (click telemetry)
- Queried `UrlClickEvents` for the scoped set.
- Found:
  - **Click events: 2**
  - **IsClickedThrough: 0** (click registered, but final redirect was blocked)
- No evidence of successful compromise from the click telemetry reviewed.

## Evidence (Sanitized)
- Stable pivot: `mandrillapp.com/track/click/31541291`
- SES-linked indicator: message identifiers ending with `amazonses.com>`
- Scope: 104 related emails on 2026-02-24
- Click telemetry: 2 clicks, `IsClickedThrough = 0`

## Outcome
- Produced a campaign-level summary for leadership:
  - pivot method (Mandrill ID),
  - scope (104),
  - delivery outcomes (some Inbox),
  - impact check (2 clicks, blocked),
  - recommendation: prioritize hunting/controls based on **stable identifiers** + delivery outcomes.

## Impact
- Shifted workflow from “single email triage” → **repeatable campaign scoping**.
- Built a reusable approach for “high-reputation platform used for bad traffic” scenarios:
  - Don’t block the entire platform.
  - Hunt using stable fingerprints + validate via telemetry.

## Lessons Learned
- Tracking IDs and infrastructure signatures can be more reliable than subject keywords.
- “Pass-ish” authentication (e.g., BestGuessPass) doesn’t equal safe.
- Always pair detection with **delivery + click** evidence before escalating impact.

## Next Steps
- Build additional pivot library:
  - common tracking patterns,
  - platform indicators (SES, marketing tools),
  - reusable KQL templates.
- Add automated reporting for “Inbox-delivered + suspicious tracking patterns.”

---

## KQL Used (Sanitized)

### Query A — SES + Mandrill linkage + delivery outcome
```kql
EmailUrlInfo
| where Url has "mandrillapp.com/track/click/"
| join kind=inner (
    EmailEvents
    | where InternetMessageId endswith "amazonses.com>"
) on NetworkMessageId
| project Timestamp, LatestDeliveryLocation, Subject, SenderMailFromAddress, InternetMessageId, Url, DeliveryAction, ConfidenceLevel
| order by Timestamp desc
```

### Query B — Click telemetry validation
```kql
EmailUrlInfo
| where Url has "mandrillapp.com/track/click/31541291"
| join kind=inner (
    EmailEvents
    | where InternetMessageId endswith "amazonses.com>"
) on NetworkMessageId
| join kind=inner UrlClickEvents on NetworkMessageId
| project Timestamp, AccountUpn, Url, ActionType, IsClickedThrough, IPAddress, UserAgent
| order by Timestamp desc
```

### Optional sanity checks (when results change day-to-day)
```kql
EmailEvents
| where Timestamp > ago(30d)
| where InternetMessageId endswith "amazonses.com>"
| summarize Count=count() by bin(Timestamp, 1d)
| order by Timestamp desc
```

```kql
EmailUrlInfo
| where Timestamp > ago(30d)
| where Url has "mandrillapp.com/track/click/"
| summarize Count=count() by bin(Timestamp, 1d)
| order by Timestamp desc
```

---

## Sanitization Note
Internal tenant identifiers, ticket numbers, raw headers, and sensitive user details are omitted or generalized.
