# Japanese Spear Phishing → ASN 29873 Pivot → Account Containment (3 resets)

_Sanitized portfolio case study (ISS/ITS) — March 3, 2026_

## Context
We identified a **Japanese-language spear phishing email** sent from a **compromised legitimate business account** (`info@seyikaku.com`).  
A key IOC in the email body was `protomed.org`.

## Problem
Answer, quickly and defensibly:
- Is this a single phish or part of an infrastructure cluster?
- Can I find a stable pivot that survives domain rotation?
- Are there downstream identity indicators suggesting compromise?
- When does this become actionable vs monitor-only?

## Constraints
- Shared hosting ASNs can contain mixed legitimate + abused assets.
- URL scanners can be evaded (different responses for bots vs humans).
- Identity “geo weirdness” can be benign; action requires corroboration.

## Actions Taken
### 1) URL analysis (behavior, not just reputation)
- Observed **multi-stage redirect** behavior.
- At the final stage, the infrastructure returned **HTTP 429** (“Google Sorry Index”-style) when it detected scanning/bot-like traffic → indicates **evasion**.

### 2) Infrastructure pivoting (domain → IP space → ASN)
- Pivoted from `protomed.org` into infrastructure artifacts.
- Repeated observations clustered in **66.96.x.x** space (Jacksonville, FL across multiple observations).
- Mapped the footprint to **ASN 29873 (HIZLAND-SD / Newfold Digital)**.
- Identified **14 related domains** in the same ASN footprint (mixed set), including examples of:
  - compromised legitimate sites used as infrastructure/launch points (e.g., `mlbinc.com`, `avitasuitestorrance.com`)
  - an impersonation-style domain (`drugcrisisinourbackyard.com`)
  - weak hygiene example: DKIM “test mode” on `bellyroles.com`

### 3) Stable header pivot (eigbox.net)
- While reviewing messages tied to the impersonation domain, found **eigbox.net** recurring as a relay artifact.
- Noted variability:
  - sometimes in **Return-Path**
  - sometimes only inside **InternetMessageId**
- Treated **eigbox.net** as a stable pivot to broaden hunting beyond sender domain.

### 4) Recipient → sign-in review (infra → identity correlation)
- Used the infrastructure/header pivots to find additional delivered mail.
- Pivoted from those messages to recipient accounts and reviewed ~30 days of sign-in activity.
- Observed unusual patterns across multiple users:
  - sign-ins across many geographies/US locations
  - frequent failures mixed with successes (or success followed by failures)
  - patterns inconsistent with typical single-region/single-device usage

## Evidence (Sanitized)
Correlated signals across layers:
- spear phish context (compromised legitimate sender)
- evasive redirect behavior (429 on scans)
- infrastructure clustering (ASN 29873 / 66.96.x.x)
- header artifact pivot (eigbox.net)
- downstream authentication anomalies (multi-geo + failure/success patterns)

## Outcome
- Escalated findings to leadership (Jonathan / Amy).
- **3 user accounts were processed for password resets** as immediate protective action.

## Impact
- Contained potential compromise quickly while continuing validation.
- Produced reusable pivots for future hunting:
  - ASN-based clustering
  - header artifact pivoting (eigbox.net)
  - infra → identity correlation workflow

## Lessons Learned
- Infrastructure pivots can outlast domain rotation.
- Relay/header artifacts can be durable pivots.
- Escalation is strongest when multiple layers align (email + infra + identity), not a single “weird” signal.

## Next Steps
- Hunt for **eigbox.net** at scale across headers (Return-Path + InternetMessageId).
- Expand ASN 29873 pivoting for new related domains and repeated IP reuse.
- Continue reviewing exposed recipients for additional containment needs.
- Consolidate IOCs (domains, URLs, IP ranges, header patterns) into reusable hunt logic.

## Sanitization Note
Internal tenant identifiers, raw headers, and sensitive user details are omitted or generalized.
