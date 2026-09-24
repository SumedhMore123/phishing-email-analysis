# Phishing Investigations

This directory contains three structured phishing investigations performed in controlled security-training environments.

Each case follows the same analyst-oriented structure so that findings can be compared consistently.

## Case Structure

Every investigation covers:

1. Investigation summary
2. Initial email details
3. Phishing indicators and social-engineering observations
4. Header / sender / domain analysis
5. URL and attachment analysis
6. Threat-intelligence findings
7. Structured IOC extraction
8. Analyst assessment
9. Recommended SOC response
10. Detection opportunities
11. Evidence and safety notes

Each case also contains an `iocs.csv` file with the indicators extracted during the investigation.

## Analyst Standard

The reports distinguish:

- **Observed evidence** — directly identified in the training scenario or analysis tool
- **Assessment** — the conclusion drawn from the combined evidence
- **Response recommendation** — actions a SOC could take after appropriate validation

A published SPF, DKIM, or DMARC record, an IP ownership result, or a single threat-intelligence detection should not be treated as conclusive on its own.

## Safety

Do not open or execute potentially malicious attachments or URLs outside an isolated analysis environment.

When publishing indicators, use defanged forms such as:

```text
hxxps://example[.]com
user[@]example[.]com
```

The repository is intended for defensive analysis and portfolio demonstration.
