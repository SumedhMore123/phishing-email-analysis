# Case 02 — Suspicious SWIFT Transfer Email

**Case status:** ✅ Complete

## 1. Investigation Summary

A sales executive at Greenholt PLC reported a suspicious email received from a known customer. The message requested a money transfer-related action and included an unsolicited attachment, which was inconsistent with the customer's normal communication style.

The investigation covered the visible email, sender/reply-to details, message-source findings, domain authentication records, attachment metadata, and VirusTotal results.

**Current assessment:** Malicious / phishing-related activity.

## 2. Initial Email Details

| Field | Observation |
|---|---|
| Display name | Mr. James Jackson |
| Sender | `info@mutawamarine.com` |
| Reply-To | `info.mutawamarine@mail.com` |
| Recipient | `webmaster@redacted.org` |
| Subject | `webmaster@redacted.org your: Transfer Reference Number:(09674321)` |
| Transfer reference | `09674321` |
| Attachment | `SWT_#09674321____PDF___CAB` |
| Stated transfer method | SWIFT |

## 3. Suspicious Indicators

### Social engineering

The email claims that funds have already been transferred and provides a payment reference, creating a business/financial context that can pressure the recipient to trust the message and open the attachment.

The reported communication was also inconsistent with the customer's usual style, according to the employee who escalated it.

### Sender / Reply-To mismatch

The visible sender is:

```text
info@mutawamarine.com
```

The Reply-To address is:

```text
info.mutawamarine@mail.com
```

This mismatch is an investigation lead because replies would be directed to a different domain than the sender address.

## 4. Header and Domain Analysis

### Originating IP

The originating IP identified in the lab was:

```text
192.119.71.157
```

The ownership associated with the originating IP was identified as:

```text
HostPapa
```

### SPF

The SPF record identified during the investigation was:

```text
v=spf1 include:spf.protection.outlook.com -all
```

### DMARC

The DMARC record identified during the investigation was:

```text
v=DMARC1; p=quarantine; fo=1
```

Authentication records should be interpreted together with the actual message headers and alignment results. A published SPF or DMARC record does not by itself prove that a particular message is legitimate.

## 5. Attachment Analysis

### Attachment

```text
SWT_#09674321____PDF___CAB
```

Although the attachment was presented with a PDF/CAB-style name, the investigated file's actual type was identified as:

```text
RAR
```

The attachment size observed in VirusTotal was approximately **400.26 KB**.

### SHA-256

```text
2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f
```

### VirusTotal

VirusTotal showed:

```text
49 / 64 security vendors flagged the file as malicious
```

The page also showed threat labels including Trojan/Ransomware and family labels such as Loki/Agensla.

These detections provide strong evidence that the attachment is malicious, while individual vendor family names should be treated as vendor-specific classifications rather than definitive attribution.

## 6. Indicators of Interest

See [`iocs.csv`](iocs.csv) for the structured indicator list.

| Type | Indicator | Context |
|---|---|---|
| Email | `info@mutawamarine.com` | Sender |
| Email | `info.mutawamarine@mail.com` | Reply-To |
| Domain | `mutawamarine.com` | Sender domain |
| Domain | `mail.com` | Reply-To domain |
| IP | `192.119.71.157` | Originating IP |
| File | `SWT_#09674321____PDF___CAB` | Attachment |
| File Type | `RAR` | Actual detected file type |
| SHA-256 | `2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f` | Attachment hash |
| Reference | `09674321` | Transfer reference |

## 7. Analyst Assessment

**Assessment: Malicious / phishing-related email.**

The assessment is supported by multiple independent indicators:

- Unexpected financial-transfer context
- Unsolicited attachment
- Sender and Reply-To domain mismatch
- Originating infrastructure requiring investigation
- Attachment disguised with a misleading file naming convention
- Actual file type identified as RAR rather than a conventional PDF
- 49/64 VirusTotal detections
- Vendor classifications indicating malicious behavior, including Trojan/Ransomware labels

The combination of message-level social engineering and a malicious attachment makes this a significant email-security event.

## 8. Recommended SOC Response

1. Quarantine the message and prevent further delivery where appropriate.
2. Search mail telemetry for the sender, Reply-To address, subject, transfer reference, and attachment hash.
3. Identify all recipients who received the same attachment or message.
4. Block confirmed malicious attachment hashes and associated indicators after validation.
5. Determine whether any recipient opened/executed the attachment.
6. Investigate endpoint telemetry for processes, network connections, persistence, or credential access associated with the attachment.
7. Escalate to incident response if execution or user interaction is confirmed.
8. Preserve the original email, headers, attachment hash, and investigation evidence.

## 9. Detection Opportunities

Potential detection logic derived from this case includes:

- Alert on sender/Reply-To domain mismatches in financial or payment-themed emails.
- Flag executable/archive attachments that use misleading document-oriented filenames.
- Detect repeated delivery of the same attachment hash across mailboxes.
- Enrich attachment hashes with malware-intelligence services.
- Correlate suspicious email delivery with endpoint execution and network telemetry.

## 10. Evidence Notes

This case is based on a controlled security-training scenario. The VirusTotal detections and vendor labels shown in the lab are evidence about the submitted sample, not independent attribution to a malware family or threat actor.

Malicious artifacts should be handled only in controlled analysis environments.
