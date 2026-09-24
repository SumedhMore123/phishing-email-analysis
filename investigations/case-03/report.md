# Case 03 — SwiftSpend Financial Phishing Campaign

**Case status:** ✅ Complete

## 1. Investigation Summary

A multi-user phishing incident was escalated after employees at SwiftSpend Financial reported suspicious emails and some users lost access to their accounts after submitting credentials.

The investigation focused on identifying the phishing infrastructure, attachment characteristics, impersonated service, exposed phishing-kit artifacts, and evidence of credential collection.

**Current assessment:** Malicious phishing campaign / credential-harvesting activity.

## 2. Attack Scenario

The reported emails were unusual for normal business communication and included phishing content and attachments intended to direct users toward an attacker-controlled Microsoft-themed login page.

The investigation demonstrated a broader campaign rather than a single isolated email, including:

- Multiple phishing emails sent to different employees
- A phishing attachment containing a redirect
- A Microsoft-impersonating login page
- A downloadable phishing-kit archive
- Evidence of credential submissions
- An email address used by the adversary to collect compromised credentials

## 3. Email Investigation

### Identified recipient

One investigated message regarding a **Quote for Services Rendered** was received by:

```text
William McClean
```

### Phishing sender

The phishing emails were sent using:

```text
Accounts.Payable@groupmarketingonline.icu
```

The use of a finance/accounting-themed mailbox is relevant to the business context of the phishing campaign.

## 4. Phishing Attachment & URL Analysis

An attachment associated with an email addressed to **Zoe Duncan** contained a redirection URL.

### Root domain

```text
kennaroads.buzz
```

### Impersonated service

The phishing login page impersonated:

```text
Microsoft
```

### Exposed archive

The investigation identified an archive exposed under the phishing infrastructure:

```text
Update365.zip
```

The archive was treated as a phishing-kit artifact for analysis.

## 5. Phishing Kit Analysis

### SHA-256

```text
ba3c15267393419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686
```

### Threat classification

The archive was classified with the following categories in the investigation:

```text
Phishing
Trojan
```

### Archive contents

VirusTotal indicated that the archive contained:

```text
49 files
```

This supports treating the archive as a phishing-kit artifact rather than an ordinary user document.

## 6. Credential Harvesting Evidence

The investigation of the `/data/Update365/` directory showed credential-submission activity.

The user identified as having submitted credentials more than once was:

```text
michael.ascot@swiftspend.finance
```

The extracted phishing-kit `submit.php` file used the following email address to collect compromised credentials:

```text
m3npat@yandex.com
```

This establishes a direct credential-collection artifact within the phishing kit.

## 7. Indicators of Interest

See [`iocs.csv`](iocs.csv) for the structured indicator inventory.

| Type | Indicator | Context |
|---|---|---|
| Email | `Accounts.Payable@groupmarketingonline.icu` | Phishing sender |
| Domain | `groupmarketingonline.icu` | Sender domain |
| Domain | `kennaroads.buzz` | Root domain of redirect URL in attachment |
| Service | Microsoft | Impersonated login provider |
| Archive | `Update365.zip` | Exposed phishing-kit archive |
| SHA-256 | `ba3c15267393419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686` | Phishing-kit archive hash |
| Threat Category | Phishing | VirusTotal classification |
| Threat Category | Trojan | VirusTotal classification |
| Archive Contents | 49 files | Files reported inside the archive |
| Email | `michael.ascot@swiftspend.finance` | User who submitted credentials more than once |
| Email | `m3npat@yandex.com` | Credential-collection address found in `submit.php` |
| User | William McClean | Recipient of Quote for Services Rendered email |

## 8. Analyst Assessment

**Assessment: Malicious phishing campaign with credential-harvesting infrastructure.**

The assessment is supported by the combination of:

- Business-themed phishing emails distributed to multiple employees
- A finance-themed sender address
- A redirect domain unrelated to the impersonated service
- A Microsoft-themed credential-harvesting page
- An exposed phishing-kit archive
- VirusTotal classification of the archive as phishing and Trojan
- 49 files contained within the archive
- Evidence that a user submitted credentials more than once
- A `submit.php` component configured to collect compromised credentials

The available evidence indicates that the attacker was operating a credential-harvesting phishing campaign and exposed portions of the supporting phishing infrastructure.

## 9. Recommended SOC Response

1. Quarantine and remove identified phishing messages from affected mailboxes.
2. Search enterprise mail telemetry for the sender address, sender domain, redirect domain, archive hash, and related subjects.
3. Identify every recipient and determine whether the phishing link or attachment was accessed.
4. Reset credentials for users who submitted credentials and invalidate active sessions where appropriate.
5. Enforce or re-verify MFA for affected accounts.
6. Investigate identity-provider logs for suspicious authentication activity after credential submission.
7. Block confirmed malicious domains, URLs, and file hashes after validation.
8. Preserve the phishing email, headers, artifacts, and credential-harvesting infrastructure for incident-response escalation.

## 10. Detection Opportunities

Potential detection logic derived from this investigation includes:

- Alert on finance/payment-themed emails from newly observed or externally hosted domains.
- Detect sender domains that differ from the organization or expected customer domains.
- Detect emails containing redirects to suspicious high-risk or unrelated root domains.
- Detect Microsoft-themed login pages hosted outside expected Microsoft infrastructure.
- Detect archive attachments and filenames associated with known phishing kits.
- Alert on repeated authentication failures or successful logins following suspected credential-phishing events.
- Search for repeated delivery of identical attachment hashes across mailboxes.
- Correlate email telemetry with identity-provider and endpoint telemetry.

## 11. Evidence Notes

This case is based on a controlled security-training scenario and the investigation results shown in the lab environment.

The VirusTotal threat categories and file-count observations are recorded as evidence from the lab. They should not be treated as independent attribution to a specific threat actor.

Malicious URLs, archives, and phishing-kit artifacts should only be accessed and handled inside controlled analysis environments.
