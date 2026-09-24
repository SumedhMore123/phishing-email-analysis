# Case 01 — Netflix-Themed Phishing Email & Attachment Analysis

**Case status:** ✅ Complete

## 1. Investigation Summary

A suspicious email reported by an end user was analyzed from a Level 1 SOC analyst perspective. The message impersonates **Netflix** and uses an account/payment issue to encourage the recipient to act.

The investigation covered email content, message headers, shortened-URL redirects, a PDF attachment, and a separate sandbox analysis of a suspicious Excel attachment associated with the phishing-analysis training scenario.

**Current assessment:** Suspicious activity / phishing.

## 2. Initial Email Details

| Field | Observation |
|---|---|
| Impersonated brand | Netflix |
| Intended recipient | `redacted@yahoo.com` |
| Subject | `YourNetflixAccountisonHold` |
| Reported classification | Suspicious activity |
| PDF attachment | `Payment-updateid.pdf` |

## 3. Phishing Indicators

### Brand impersonation

The email uses Netflix branding and presents an account/payment problem designed to encourage the recipient to take action.

### Sender / domain inconsistency

The message's `Return-Path` uses the domain `etekno.xyz`, which does not match the impersonated Netflix brand.

The observed authentication results include SPF `none`, DKIM `unknown`, and DMARC `unknown`.

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

The multiple redirect layers warrant investigation. The final LinkedIn URL is recorded as the final observed destination and is not labeled malicious by itself.

## 4. Header Analysis

### Received From

```text
209.85.167.226
```

The relevant `Received:` header identifies this hop as `mail-oi1-f226.google.com`.

> This IP is recorded as a header indicator. It should not automatically be treated as attacker infrastructure without additional attribution evidence.

### Return-Path

```text
postmaster@etekno.xyz
```

**Domain of interest:** `etekno.xyz`

## 5. Attachment Analysis — PDF

The suspicious email contains:

```text
Payment-updateid.pdf
```

The analysis environment classified the attachment activity as **Suspicious activity**.

### SHA-256

```text
cc6f1a04b10bcb168aeec8d870b97bd7c20fc161e8310b5bce1af8ed420e2c24
```

### Sandbox observations

The sandbox report showed activity involving Adobe Reader processes while opening the PDF:

```text
Process: AcroRd32.exe
Flagged IP: 2.16.107.24
Potentially Bad Traffic process: svchost.exe
```

These observations should be treated as sandbox evidence associated with the sample and investigated further rather than used alone for attribution.

## 6. Additional Attachment Analysis — Excel Sample

This Excel sample was analyzed separately within the same controlled training scenario and is kept distinct from the PDF evidence above to avoid conflating two artifacts.

A separate ANY.RUN sandbox analysis in the phishing-analysis training scenario examined an Excel attachment.

| Field | Observation |
|---|---|
| File type | `.xlsx` |
| ANY.RUN classification | Malicious activity |
| Filename | `CBJ200620039539.xlsx` |
| SHA-256 | `5f94a66e0ce78d17afc2dd27fc17b44b3ffc13ac5f42d3ad6a5dcfb36715f3eb` |
| Malicious domain | `biz9holdings.com` |
| Associated IP | `204.11.56.48` |
| Other malicious domain | `findresults.site` |
| Exploit target | `CVE-2017-11882` |

The sandbox screenshot shows Microsoft Excel 2010 opening the sample while ANY.RUN classifies the attachment as **Malicious activity**.

The observed attachment also attempts to exploit **CVE-2017-11882**, providing a strong indicator of weaponized document behavior in the sandbox scenario.

## 7. Indicators of Interest

See [`iocs.csv`](iocs.csv) for the structured indicator list.

### Email / PDF indicators

| Type | Indicator | Context |
|---|---|---|
| Domain | `etekno.xyz` | Return-Path domain |
| IP | `209.85.167.226` | `Received: from` header hop |
| IP | `2.16.107.24` | IP flagged in PDF sandbox report |
| URL | `https://t.co/yuxfZm8KPg?amp=1` | Email CTA / shortened URL |
| URL | `https://bit.ly/2TiRpyj` | Redirect destination |
| URL | `https://www.linkedin.com/slink?code=enmd-V3` | Final observed URL |
| SHA-256 | `cc6f1a04b10bcb168aeec8d870b97bd7c20fc161e8310b5bce1af8ed420e2c24` | PDF attachment |
| File | `Payment-updateid.pdf` | Email attachment |

### Excel attachment indicators

| Type | Indicator | Context |
|---|---|---|
| File | `CBJ200620039539.xlsx` | Suspicious Excel attachment |
| SHA-256 | `5f94a66e0ce78d17afc2dd27fc17b44b3ffc13ac5f42d3ad6a5dcfb36715f3eb` | Excel attachment |
| Domain | `biz9holdings.com` | Malicious domain observed in sandbox |
| IP | `204.11.56.48` | IP associated with malicious domain |
| Domain | `findresults.site` | Additional malicious domain |
| CVE | `CVE-2017-11882` | Exploitation target |

## 8. Analyst Verdict

**Verdict: Suspicious / Phishing**

The assessment is based on the combined evidence: brand impersonation, account/payment urgency, sender-domain inconsistency, missing/unknown email authentication results, multi-stage URL redirection, suspicious PDF behavior, and malicious attachment behavior observed in sandbox analysis.

The evidence supports treating the message and associated artifacts as a phishing-related security event requiring investigation and appropriate containment.

## 9. Recommended SOC Response

For a real enterprise environment, a Level 1 analyst could:

1. Quarantine the reported message and prevent further delivery where appropriate.
2. Search mail telemetry for the same sender, subject, URLs, domains, filenames, and hashes.
3. Determine whether other users received the message or related campaign artifacts.
4. Block confirmed malicious indicators after validation.
5. Check whether recipients clicked the URL or opened attachments.
6. If credentials may have been submitted, follow the organization's credential-reset and account-review process.
7. Investigate endpoint and identity telemetry following email interaction.
8. Preserve the original email, headers, hashes, network indicators, and sandbox evidence for escalation.

## 10. Detection Opportunities

Potential detection logic derived from this investigation includes:

- Alert on external emails where the displayed brand and sender/Return-Path domains are inconsistent.
- Detect high-risk account/payment emails containing URL-shortening services.
- Detect repeated delivery of the same attachment hash across mailboxes.
- Enrich extracted domains, URLs, IPs, and file hashes with threat-intelligence data.
- Detect Office documents associated with known exploit CVEs or suspicious child-process/network behavior.
- Correlate phishing-email interactions with subsequent suspicious authentication or endpoint activity.

## 11. Evidence Notes

This case is based on a controlled security-training scenario and sandbox observations. Indicators should be validated before being operationalized as production block rules.

Malicious or suspicious URLs should be handled safely and defanged when published in contexts where accidental execution is possible.
