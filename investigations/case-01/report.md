# Case 01 — Netflix-Themed Account Update Email

## 1. Investigation Summary

A suspicious email reported by an end user was analyzed from a Level 1 SOC analyst perspective. The message impersonates **Netflix** and asks the recipient to update payment/account information.

The investigation examined the visible email, message headers, URL redirects, and the attached PDF in the provided analysis environment.

**Current assessment:** Suspicious activity / phishing.

## 2. Initial Email Details

| Field | Observation |
|---|---|
| Impersonated brand | Netflix |
| Intended recipient | `redacted@yahoo.com` |
| Subject | `YourNetflixAccountisonHold` |
| Reported classification | Suspicious activity |
| Attachment | `Payment-updateid.pdf` |

## 3. Phishing Indicators

### Brand impersonation

The email uses Netflix branding and presents an account/payment problem designed to encourage the recipient to take immediate action.

### Sender / domain inconsistency

The message's `Return-Path` uses the domain `etekno.xyz`, which does not match the impersonated Netflix brand.

The relevant header authentication observations include SPF `none`, DKIM `unknown`, and DMARC `unknown`.

### URL redirection

The `UPDATE ACCOUNT NOW` button uses a shortened URL. The observed redirect chain was:

```text
https://t.co/yuxfZm8KPg?amp=1
        |
        | HTTP 301
        v
https://bit.ly/2TiRpyj
        |
        | HTTP 301
        v
https://www.linkedin.com/slink?code=enmd-V3
        |
        | 0 additional redirects observed
        v
Final observed destination
```

The use of multiple redirectors hides the destination from the recipient and warrants investigation. The final LinkedIn URL should not itself be labeled malicious without additional evidence.

## 4. Header Analysis

### Received From

```text
209.85.167.226
```

The header identifies the sending hop as `mail-oi1-f226.google.com`.

> The IP is recorded as a header indicator. It should not automatically be treated as the attacker's infrastructure without additional attribution evidence.

### Return-Path

```text
postmaster@etekno.xyz
```

**Domain of interest:** `etekno.xyz`

## 5. Attachment Analysis

The suspicious email contains the following PDF attachment:

```text
Payment-updateid.pdf
```

The analysis environment classified the attachment activity as **Suspicious activity**.

### SHA-256

```text
cc6f1a04b10bcb168aeec8d870b97bd7c20fc161e8310b5bce1af8ed420e2c24
```

### Sandbox observations

The sandbox report showed activity involving Adobe Reader processes while opening the PDF. The analysis also identified:

```text
Process: AcroRd32.exe
Flagged IP: 2.16.107.24
Potentially Bad Traffic process: svchost.exe
```

These observations should be treated as sandbox evidence associated with the sample and investigated further rather than used alone to establish attribution.

## 6. Indicators of Interest

See [`iocs.csv`](iocs.csv) for the structured indicator list.

| Type | Indicator | Context |
|---|---|---|
| Domain | `etekno.xyz` | Return-Path domain |
| IP | `209.85.167.226` | `Received: from` header hop |
| IP | `2.16.107.24` | IP flagged in sandbox report |
| URL | `https://t.co/yuxfZm8KPg?amp=1` | Email CTA / shortened URL |
| URL | `https://bit.ly/2TiRpyj` | Redirect destination |
| URL | `https://www.linkedin.com/slink?code=enmd-V3` | Final observed URL |
| SHA-256 | `cc6f1a04b10bcb168aeec8d870b97bd7c20fc161e8310b5bce1af8ed420e2c24` | PDF attachment |
| File | `Payment-updateid.pdf` | Email attachment |

## 7. Analyst Verdict

**Verdict: Suspicious / Phishing**

The classification is based on the combination of:

- Netflix brand impersonation
- Account/payment urgency
- Return-Path domain mismatch (`etekno.xyz`)
- Missing/unknown email authentication results in the observed headers
- A shortened URL with multiple redirects
- A suspicious PDF attachment
- Sandbox observations showing suspicious activity

The evidence supports treating the message as a phishing-related security event requiring investigation and appropriate containment.

## 8. Recommended SOC Response

For a real enterprise environment, a Level 1 analyst could:

1. Quarantine the reported message and prevent further delivery where appropriate.
2. Search mail telemetry for the same sender, subject, URLs, domain, and attachment hash.
3. Determine whether other users received the message.
4. Block confirmed malicious indicators after validation.
5. Check whether the recipient clicked the URL or opened the attachment.
6. If credentials may have been submitted, initiate the organization's credential-reset and account-review procedure.
7. Investigate endpoint and identity telemetry for activity following email interaction.
8. Preserve the original email, headers, attachment hash, and investigation evidence for escalation.

## 9. Detection Opportunities

Potential detection logic derived from this investigation includes:

- Alert on external emails where the displayed brand and sender/Return-Path domains are inconsistent.
- Detect known or suspicious URL-shortening services in high-risk account/payment emails.
- Detect repeated delivery of the same attachment hash across mailboxes.
- Enrich extracted domains, URLs, IPs, and file hashes with threat-intelligence data.
- Correlate phishing-email interactions with subsequent suspicious authentication or endpoint activity.

## 10. Evidence Notes

This case is based on a controlled security-training scenario and sandbox observations. Indicators should be validated before being operationalized as production block rules.

Malicious or suspicious URLs should be handled safely and defanged when published in contexts where accidental execution is possible.
