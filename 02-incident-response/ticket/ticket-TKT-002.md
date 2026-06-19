# TKT-002 | Suspicious Phishing Email With Malicious Attachment

| Ticket ID | TKT-002 |
|---|---|
| Date & Time | August 24, 2017, 3:27 AM |
| Severity (Critical / High / Medium / Low) | HIGH |
| Kill Chain Phase | Delivery |
| MITRE ATT&CK Technique(s) | T1566.001 · T1203 |
| Incident Report | [triage-report_SuspiciousPhishingEmailWithMaliciousAttachment.md](../02-incident-response/incident-report/triage-report_severity_HIGH.md) |

## SCENARIO

A Frothly employee reported receiving a suspicious email containing a password-protected attachment. The mail gateway flagged the message and SPF verification for the sender failed; the sender domain appears recently registered. Your task is to investigate the email metadata, identify the recipient and attachment details, determine whether the user interacted with the file, and assess any resulting endpoint activity or malicious execution.
