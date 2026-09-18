# Phishing Email Analysis — Prevention & Email Security Notes

This section captures defensive concepts practiced in the TryHackMe **Phishing Prevention** room and connects them to the SOC investigation workflow used in this repository.

## Topics Covered

- **SPF (Sender Policy Framework)** — sender-domain authentication using authorized mail servers
- **DKIM (DomainKeys Identified Mail)** — cryptographic signing of email messages
- **DMARC (Domain-based Message Authentication, Reporting, and Conformance)** — policy and reporting layer built around SPF/DKIM alignment
- **S/MIME** — mechanisms for email signing and encryption
- **SMTP response analysis** — using response codes and message text to identify rejected or blocked mail
- **Wireshark SMTP analysis** — filtering and examining SMTP traffic in packet captures
- **IMF (Internet Message Format)** — examining sender, recipient, content, and attachment metadata
- **Attachment analysis** — identifying potentially malicious files and their encoding
- **Phishing prevention controls** — reducing delivery and user-impact risk through layered email security controls

## SOC Relevance

These topics complement the investigation workflow in `investigations/`.

During phishing triage, email authentication results such as SPF, DKIM, and DMARC can provide useful evidence about sender authenticity and domain alignment. SMTP response codes and packet-level analysis can help determine whether messages were accepted, rejected, or blocked. IMF analysis provides visibility into message metadata and attachments.

The objective is not to treat one signal as conclusive. A SOC analyst should correlate authentication results, header fields, message content, URLs, attachments, threat-intelligence findings, and endpoint/network telemetry before reaching a final assessment.

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

## Example Evidence Types

| Evidence source | Useful investigation data |
|---|---|
| SPF / DKIM / DMARC | Authentication and domain-alignment observations |
| SMTP response | Delivery/rejection status and server response text |
| Wireshark PCAP | SMTP conversations, response codes, IPs, message flow |
| IMF | Sender, recipient, MIME structure, attachment metadata |
| Attachment analysis | Filename, hash, execution behavior, network indicators |

## Training Note

The prevention concepts in this document are based on hands-on security training and are used here to support the practical phishing investigations documented elsewhere in the repository.
