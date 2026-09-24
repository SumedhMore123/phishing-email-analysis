# Phishing Prevention & Email Security

This section captures defensive concepts practiced during security training and connects them to the phishing investigations in this repository.

---

## Topics Covered

- **SPF (Sender Policy Framework)** — sender-domain authentication using authorized mail servers
- **DKIM (DomainKeys Identified Mail)** — cryptographic signing of email messages
- **DMARC (Domain-based Message Authentication, Reporting, and Conformance)** — policy and reporting layer built around SPF/DKIM alignment
- **S/MIME** — mechanisms for email signing and encryption
- **SMTP response analysis** — using response codes and message text to identify rejected or blocked mail
- **Wireshark SMTP analysis** — filtering and examining SMTP traffic in packet captures
- **IMF (Internet Message Format)** — examining sender, recipient, content, and attachment metadata
- **Attachment analysis** — identifying potentially malicious files and their characteristics
- **Layered phishing prevention** — reducing delivery and user-impact risk through multiple controls

---

## SOC Relevance

During phishing triage, SPF, DKIM, and DMARC results can provide evidence about sender authenticity and domain alignment. SMTP response codes and packet-level analysis can help determine how a message was handled. IMF analysis provides visibility into message metadata and attachments.

These signals should be correlated with:

- Sender / Reply-To information
- Message content
- URLs
- Attachment characteristics
- Threat-intelligence results
- Endpoint and identity telemetry

No single authentication or reputation result should be treated as conclusive on its own.

---

## Practical Investigation Workflow

```text
Suspicious Email
      ↓
Header / Authentication Analysis
      ↓
Sender & Domain Validation
      ↓
URL / Attachment Analysis
      ↓
SMTP / Network Evidence (when available)
      ↓
IOC Extraction
      ↓
Detection & Response
```

---

## Example Evidence Types

| Evidence source | Useful investigation data |
|---|---|
| SPF / DKIM / DMARC | Authentication and domain-alignment observations |
| SMTP response | Delivery/rejection status and server response text |
| Wireshark PCAP | SMTP conversations, response codes, IPs, message flow |
| IMF | Sender, recipient, MIME structure, attachment metadata |
| Attachment analysis | Filename, hash, file type, execution behavior, network indicators |

---

## Training Note

The concepts in this document are used as defensive foundations for the practical phishing investigations documented elsewhere in the repository. Production controls should be implemented and tuned according to organizational policy and environment.
