# TKT-009 | Brute-Force Authentication Attack on RDP — Successful Logon

| Ticket ID | TKT-009 |
|---|---|
| Date & Time | June 29, 2017, 10:51 AM |
| Severity (Critical / High / Medium / Low) | MEDIUM |
| Kill Chain Phase | Exploitation |
| MITRE ATT&CK Technique(s) | T1110.001 · T1078 |
| Incident Report | [triage-report_DomainController_Compromise.md](../incident-report/triage-report_severity_MEDIUM.md) |

## SCENARIO

Windows Security logs on a Frothly Domain Controller recorded hundreds of consecutive failed logon attempts against the Administrator account from a single external IP address late at night. The attempts were followed by a successful logon from the same IP using Remote Desktop Protocol. A successful RDP logon to a Domain Controller by an external IP is a critical severity incident requiring immediate escalation.
