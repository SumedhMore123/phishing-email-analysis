# Splunk Queries

The searches below are **illustrative SPL examples** showing how the investigation patterns in this project can be operationalized in a SIEM.

> **Important:** Field names such as `sender`, `reply_to`, `recipient`, `subject`, `body`, `attachment_name`, and `attachment_sha256` are generic examples. Adapt them to the organization's actual email/SIEM schema and test them before production use.

---

## 1. Sender / Reply-To Domain Mismatch

```spl
index=email
| eval sender_domain=lower(replace(sender, "^.*@", ""))
| eval reply_to_domain=lower(replace(reply_to, "^.*@", ""))
| where isnotnull(sender_domain)
    AND isnotnull(reply_to_domain)
    AND sender_domain!=reply_to_domain
| table _time sender reply_to recipient subject sender_domain reply_to_domain
```

**Use:** Identify messages where replies would be directed to a different domain than the visible sender.

---

## 2. Urgent Financial / Account-Themed Messages

```spl
index=email
| eval email_text=lower(subject." ".body)
| where match(
    email_text,
    "(urgent|immediate|payment|invoice|transfer|suspend|legal action|verification|password)"
)
| table _time sender recipient subject
```

**Use:** Surface socially engineered messages for enrichment with sender reputation, attachment, and URL evidence.

---

## 3. Archive / High-Risk Attachment Filenames

```spl
index=email
| eval name=lower(attachment_name)
| where match(name, "\\.(zip|rar|7z|iso|img|js|vbs|scr|exe)$")
| table _time sender recipient subject attachment_name attachment_sha256
```

**Use:** Identify attachment types that may require additional inspection.

---

## 4. Repeated Attachment Hash Across Recipients

```spl
index=email
| where isnotnull(attachment_sha256)
| stats
    dc(recipient) as recipient_count
    values(recipient) as recipients
    values(sender) as senders
    values(subject) as subjects
    by attachment_sha256
| where recipient_count > 1
| sort - recipient_count
```

**Use:** Identify potentially campaign-wide attachment delivery.

---

## 5. Search for a Confirmed IOC

Replace the example value with a validated indicator from an investigation.

```spl
index=email
| search attachment_sha256="VALIDATED_SHA256"
    OR sender="VALIDATED_SENDER"
    OR url="VALIDATED_URL"
    OR domain="VALIDATED_DOMAIN"
| table _time sender recipient subject attachment_name attachment_sha256 url domain
```

**Use:** Scope the presence of a known indicator across mail telemetry.

---

## 6. Post-Phishing Authentication Correlation

If the environment collects email and identity telemetry in searchable datasets, correlate the affected user with authentication events after a suspected phishing interaction.

Conceptually:

```text
Email event
   ↓
Affected user / recipient
   ↓
Phishing URL or attachment interaction
   ↓
Identity telemetry
   ↓
Suspicious authentication review
```

The exact SPL depends on the organization's identity schema and available data sources.

---

## Query Tuning Notes

- Normalize email addresses and domains to lowercase.
- Account for missing Reply-To fields.
- Avoid blocking solely on keyword matches.
- Enrich with sender reputation, authentication results, attachment type, and URL reputation where available.
- Record the fields used for investigation so alerts remain explainable.

These searches demonstrate how findings from the case investigations can be translated into practical SIEM investigation workflows.
