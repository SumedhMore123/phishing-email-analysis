# Phishing Email Analysis

A practical SOC-focused portfolio project documenting phishing email investigations, indicator extraction, threat-intelligence analysis, detection opportunities, and incident-response recommendations.

## Project Objectives

- Analyze suspicious emails from a Level 1 SOC analyst perspective
- Identify phishing indicators across email content and headers
- Extract and document actionable indicators of compromise (IOCs)
- Trace URLs and redirects safely
- Analyze suspicious attachments and sandbox observations
- Translate investigation findings into detection and response opportunities

## Investigation Workflow

```text
Email Triage
    ↓
Header & Sender Analysis
    ↓
URL / Attachment Analysis
    ↓
Threat Intelligence
    ↓
IOC Extraction
    ↓
Verdict
    ↓
Detection & Incident Response
```

## Cases

| Case | Description | Status |
|---|---|---|
| [Case 01](investigations/case-01/report.md) | Netflix-themed account/payment update phishing investigation | In progress |

## Tools & Techniques

- Thunderbird Message Source
- ANY.RUN
- Email header analysis
- URL redirect analysis
- Threat intelligence and IOC enrichment
- Splunk detection queries (planned)

> **Note:** This repository documents analysis performed in controlled security-training environments. Malicious URLs are defanged where appropriate.
