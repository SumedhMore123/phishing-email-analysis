# Incident Response

This playbook provides a practical response workflow for suspected phishing emails based on the investigation patterns documented in this repository.

The exact actions should follow the organization's incident-response policy, email-security platform, identity controls, and escalation procedures.

---

## 1. Triage

### Collect

Capture:

- Original email
- Full message headers
- Sender and Reply-To addresses
- Subject and timestamps
- Recipient(s)
- URLs and redirect chain
- Attachment name and actual file type
- SHA-256 hash
- Threat-intelligence / sandbox results

### Establish the initial assessment

Classify the email using the available evidence:

```text
Suspicious
   ↓
Investigate
   ↓
Corroborate evidence
   ↓
Phishing / malicious activity confirmed
   OR
Benign / insufficient evidence
```

Avoid declaring an attachment malicious before it has been analyzed or enriched with reliable evidence.

---

## 2. Containment

Where organizational policy permits:

1. Quarantine the reported message.
2. Search for and remove matching copies from other mailboxes.
3. Disable or block confirmed malicious indicators.
4. Prevent additional delivery of the same campaign.
5. Preserve the original message and investigation artifacts.

Containment actions should be based on validated indicators to reduce the risk of blocking legitimate business traffic.

---

## 3. Scope the Campaign

Search email telemetry for:

- Sender address
- Sender domain
- Reply-To address
- Subject
- Attachment filename
- Attachment SHA-256
- URLs
- Redirect domains
- Recipient set
- Related timestamps

Determine:

- How many users received the message?
- Did multiple messages use the same infrastructure?
- Did users click the link or open the attachment?
- Are there related emails with different sender addresses or subjects?

---

## 4. Credential-Phishing Branch

When a user submitted credentials:

1. Reset or rotate the affected credentials according to policy.
2. Revoke active sessions/tokens where supported.
3. Verify or re-enforce MFA.
4. Review identity-provider logs for suspicious authentication.
5. Check for password reuse risk where organizational controls and policy permit.
6. Preserve authentication evidence for escalation.

The response should treat the account as potentially exposed until the organization completes its investigation.

---

## 5. Attachment / Endpoint Branch

When a user opened or executed a suspicious attachment:

1. Identify the affected endpoint.
2. Determine process execution and child-process activity.
3. Review file hash and associated network connections.
4. Search for persistence or other endpoint indicators.
5. Review relevant EDR / endpoint telemetry.
6. Escalate to the incident-response team if compromise is suspected or confirmed.

Potentially malicious files should only be handled in isolated analysis environments.

---

## 6. Eradication & Recovery

After the scope is understood:

- Remove malicious email copies.
- Block confirmed indicators.
- Remediate affected endpoints according to incident-response procedures.
- Reset exposed credentials and revoke compromised sessions.
- Confirm that malicious URLs, files, or domains are no longer reachable from managed systems where applicable.
- Monitor affected users and endpoints for recurrence.

---

## 7. Documentation & Closure

Record:

- Detection source
- Timeline
- Affected users
- Key evidence
- IOCs
- Analyst assessment
- Actions taken
- Escalations
- Final impact
- Lessons learned

A concise timeline is especially useful:

```text
Email received
      ↓
User interaction
      ↓
Credential / execution event
      ↓
Alert or report
      ↓
Investigation
      ↓
Containment
      ↓
Recovery
```

---

## Escalation Criteria

Escalate when evidence indicates:

- Credential submission
- Suspicious authentication after phishing interaction
- Attachment execution
- Malware behavior
- Persistence
- Multiple affected users
- Confirmed compromise
- Unusual data access or exfiltration indicators

The objective is to move from **email-level containment** to **account / endpoint / incident response** when the evidence requires it.
