# Detection Logic

This document translates recurring observations from the three investigations into detection opportunities.

The goal is not to create a single "phishing = X" rule. Effective email detection should combine multiple weak or moderate signals and then enrich or investigate the resulting event.

---

## 1. Sender / Reply-To Mismatch

### Logic

Raise an investigation signal when:

- the sender address is external,
- a Reply-To address is present,
- and the Reply-To domain differs from the sender domain.

### Why it matters

A mismatch is a useful investigation lead because replies may be redirected to infrastructure that differs from the visible sender. It is not, by itself, proof of phishing.

### Example conditions

```text
External sender
    AND
Reply-To present
    AND
Sender domain != Reply-To domain
```

---

## 2. Urgency + Financial / Account Language

Look for combinations of terms associated with social engineering:

- urgent / immediate action
- payment / invoice / transfer
- account suspension
- legal action
- password / verification
- security alert

A higher-confidence alert can be produced when this language is combined with an external sender, suspicious attachment, or unusual URL.

---

## 3. URL Shortening or Multi-Stage Redirects

Investigate messages containing URL-shortening services or redirect chains, especially when they lead to infrastructure unrelated to the claimed brand or business.

Useful signals include:

- shortened URL domains
- multiple HTTP redirects
- newly observed destination domains
- destination domains unrelated to the impersonated service

---

## 4. Attachment Masquerading

Raise a signal when a file uses a document-oriented filename but the actual file type differs.

Examples from the investigations include:

```text
Document-style filename
        +
Unexpected archive / executable type
        =
High-priority investigation lead
```

Do not rely only on the filename extension. Compare the filename, MIME/type information, file signature, and threat-intelligence results.

---

## 5. Repeated Attachment Delivery

The same attachment hash delivered to multiple recipients can indicate campaign activity.

Recommended enrichment:

- SHA-256
- filename
- sender
- subject
- recipient count
- first-seen / last-seen time
- URL/domain associations

---

## 6. Brand Impersonation

Investigate cases where:

- displayed branding claims to be a trusted service,
- sender infrastructure is unrelated or unusual,
- links lead outside expected infrastructure,
- or a credential page imitates the claimed service.

Microsoft-themed credential pages and Netflix-themed account notices are examples observed in this project.

---

## 7. Credential-Phishing Follow-On Detection

Email detection should not end at delivery.

Where telemetry is available, correlate:

```text
Phishing Email
     ↓
Link / Attachment Interaction
     ↓
Authentication Anomaly
     ↓
Session / MFA / Endpoint Investigation
```

Useful signals include unusual authentication immediately after suspected phishing interaction, repeated authentication failures, unfamiliar source locations, or suspicious endpoint activity.

---

## Detection Engineering Principles

- Use multiple signals rather than one keyword.
- Prefer enrichment and correlation over aggressive blocking.
- Tune rules against legitimate business traffic.
- Maintain allowlists carefully.
- Log the evidence that triggered the alert so an analyst can reproduce the decision.
- Validate indicators before promoting them to production block rules.

These concepts are derived from the investigation patterns documented in this repository.
