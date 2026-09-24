# Phishing Email Analysis

A practical **SOC / Blue Team portfolio project** documenting the investigation of suspicious email activity across three controlled security-training scenarios.

The project focuses on **email triage, header and sender analysis, URL and attachment investigation, threat-intelligence enrichment, IOC extraction, detection opportunities, and incident-response recommendations**.

> **Scope:** All investigations in this repository were performed in controlled security-training environments. The project is intended to demonstrate analyst methodology and documentation discipline, not production incident-handling experience.

---

## What This Project Demonstrates

- Phishing email triage from a Level 1 SOC analyst perspective
- Email header and sender analysis
- SPF / DKIM / DMARC interpretation
- URL and redirect investigation
- Suspicious attachment identification and hashing
- Sandbox and threat-intelligence evidence interpretation
- IOC extraction and structured documentation
- Credential-phishing and phishing-kit analysis
- Investigation scoping across multiple recipients
- Detection logic and Splunk search examples
- Incident-response and containment planning

---

## Investigation Workflow

```text
Email Alert / User Report
          ↓
Initial Triage
          ↓
Header & Sender Analysis
          ↓
URL / Attachment Analysis
          ↓
Threat Intelligence
          ↓
IOC Extraction
          ↓
Scope & Impact Assessment
          ↓
Analyst Verdict
          ↓
Detection & Incident Response
          ↓
Documentation
```

The workflow deliberately uses **multiple evidence sources** before reaching an assessment. A single suspicious signal is treated as an investigation lead rather than automatic proof of malicious activity.

---

## Investigations

| Case | Scenario | Primary focus | Status |
|---|---|---|---|
| [Case 01](investigations/case-01/report.md) | Netflix-themed phishing email | Brand impersonation, redirect analysis, PDF/Excel sandbox evidence | ✅ Complete |
| [Case 02](investigations/case-02/report.md) | Suspicious SWIFT transfer email | Sender/Reply-To mismatch, attachment masquerading, malware intelligence | ✅ Complete |
| [Case 03](investigations/case-03/report.md) | SwiftSpend phishing campaign | Microsoft impersonation, phishing-kit artifacts, credential harvesting | ✅ Complete |

Each investigation includes a narrative report and a structured [IOC inventory](investigations/).

---

## Detection & Response

The investigation findings were translated into reusable defensive content:

- [Detection Logic](detection/detection-logic.md) — detection ideas derived from recurring phishing indicators
- [Splunk Queries](detection/splunk-queries.md) — illustrative SPL searches for common phishing patterns
- [Incident Response Playbook](response/incident-response.md) — triage, containment, scoping, credential response, endpoint investigation, and documentation steps
- [Methodology](resources/methodology.md) — evidence-handling and investigation principles
- [Phishing Prevention](resources/phishing-prevention.md) — SPF, DKIM, DMARC, SMTP/IMF, S/MIME, and defensive email-security concepts

> Splunk searches are provided as **examples** and use generic field names. Field mappings should be adapted and tested against the target organization's email/SIEM schema before operational use.

---

## Tools & Techniques

### Investigation
- Thunderbird Message Source
- Email header analysis
- URL redirect analysis
- IOC extraction
- SHA-256 hashing

### Analysis & Threat Intelligence
- ANY.RUN
- VirusTotal
- Wireshark
- Sandbox evidence interpretation
- Domain, IP, URL, and file-hash enrichment

### Detection & Response
- Splunk SPL concepts
- Email-security triage
- Campaign scoping
- IOC-based searching
- Endpoint and identity telemetry correlation
- Containment and remediation planning

---

## Repository Structure

```text
phishing-email-analysis/
│
├── investigations/
│   ├── case-01/
│   │   ├── report.md
│   │   └── iocs.csv
│   ├── case-02/
│   │   ├── report.md
│   │   └── iocs.csv
│   ├── case-03/
│   │   ├── report.md
│   │   └── iocs.csv
│   └── README.md
│
├── detection/
│   ├── detection-logic.md
│   └── splunk-queries.md
│
├── response/
│   └── incident-response.md
│
├── resources/
│   ├── methodology.md
│   └── phishing-prevention.md
│
├── LICENSE
└── README.md
```

---

## Analyst Documentation Principles

This repository follows a few practical rules:

1. **Separate evidence from interpretation.** Observations are recorded before the analyst assessment.
2. **Correlate multiple signals.** Sender details, authentication, message content, URLs, files, and telemetry should be considered together.
3. **Do not over-attribute.** Vendor malware labels and infrastructure ownership are recorded as evidence, not automatically treated as proof of actor identity.
4. **Validate before blocking.** Production block rules should be based on confirmed indicators and organizational procedures.
5. **Preserve evidence.** Original messages, headers, hashes, URLs, and sandbox observations should be retained for escalation.
6. **Handle malicious artifacts safely.** Potentially malicious files and URLs belong in isolated analysis environments.

---

## Project Outcome

The finished project demonstrates an end-to-end phishing-analysis workflow:

**Triage → Investigation → Evidence Correlation → IOC Extraction → Scope Assessment → Verdict → Detection → Response**

The emphasis is on producing an analyst-style investigation trail that can be reviewed, reproduced, and discussed in a SOC interview.

> **Safety:** Potentially malicious URLs are defanged where appropriate. Do not open suspicious URLs or attachments on a normal workstation.
