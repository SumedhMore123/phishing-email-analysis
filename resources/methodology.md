# Methodology

This repository uses a repeatable phishing-analysis methodology designed around the workflow a Level 1 SOC analyst can use to triage and investigate suspicious email activity.

---

## 1. Preserve the Evidence

Start with the original message and message source where available.

Capture:

- Sender
- Reply-To
- Return-Path
- Received headers
- Authentication results
- Subject
- Recipient
- URLs
- Attachment names
- Attachment hashes

Do not alter the original evidence during collection.

---

## 2. Separate Observation from Assessment

Use three distinct layers:

### Observation

What the evidence directly shows.

### Interpretation

Why the observation is relevant to phishing investigation.

### Assessment

The conclusion reached after correlating multiple observations.

Example:

```text
Observation:
Reply-To domain differs from sender domain.

Interpretation:
Replies may be routed to different infrastructure.

Assessment:
Suspicious indicator requiring further validation.
```

---

## 3. Correlate Multiple Signals

Consider evidence across:

```text
Email content
    +
Header / authentication evidence
    +
URL infrastructure
    +
Attachment characteristics
    +
Threat intelligence
    +
Endpoint / identity telemetry
```

No single field should automatically determine the final assessment.

---

## 4. Analyze URLs Safely

Document:

- Original URL
- Redirect chain
- Final observed destination
- Domain relationships
- Reputation results

Never open suspicious links from a normal workstation. Use isolated analysis environments and defang published indicators where appropriate.

---

## 5. Analyze Attachments Safely

For suspicious files:

1. Record the filename.
2. Identify the actual file type.
3. Calculate SHA-256.
4. Review available reputation data.
5. Use an isolated sandbox when appropriate.
6. Record observed processes, network indicators, and classifications.
7. Distinguish tool/vendor classification from confirmed attribution.

---

## 6. Scope the Incident

Once indicators are known, search for recurrence.

Examples:

- Same sender
- Same domain
- Same subject
- Same URL
- Same attachment filename
- Same SHA-256
- Same recipient cluster
- Related authentication or endpoint activity

This determines whether the event is isolated or part of a broader campaign.

---

## 7. Extract Actionable IOCs

The repository stores indicators in `iocs.csv` using:

```text
type,value,description
```

Typical types include:

- Email
- Domain
- IP
- URL
- File
- FileType
- SHA256
- CVE
- ThreatCategory
- Reference

Contextual data that is not an actionable IOC may still be retained in the case report for investigative clarity.

---

## 8. Make the Assessment Evidence-Based

A final assessment should explain **why** the message was considered suspicious or malicious.

Strong evidence may include:

- Credential-harvesting functionality
- Malicious attachment behavior
- Consistent threat-intelligence detections
- Confirmed phishing infrastructure
- Multiple correlated phishing indicators

Weak evidence should remain described as an investigation lead rather than overstated as proof.

---

## 9. Translate Findings into Defense

The final stage converts investigation findings into:

- Detection logic
- SIEM searches
- Email-security controls
- IOC searches
- Containment steps
- Credential-response actions
- Endpoint investigation steps

This closes the loop between **analysis and operational defense**.

---

## Training Environment Note

The cases in this repository come from controlled security-training environments. Findings are documented as observed in those scenarios and should not be treated as production telemetry or as attribution to a specific threat actor.

The methodology is intended to demonstrate repeatable analyst reasoning, evidence handling, and security documentation.
