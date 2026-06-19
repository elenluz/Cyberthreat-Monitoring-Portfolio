# TKT-010 | Drive-By Download via Malicious Ad Network Redirect

| Ticket ID | TKT-010 |
|---|---|
| Date & Time | August 15, 2017, 11:35 AM |
| Severity (Critical / High / Medium / Low) | LOW |
| Kill Chain Phase | Delivery |
| MITRE ATT&CK Technique(s) | T1189 · T1204.001 |
| Incident Report | [triage-report_DriveBy_XSS_Compromise.md](../02-incident-response/triage-report_DriveBy_XSS_Compromise.md) |

## SCENARIO

Proxy and HTTP stream logs show a Frothly workstation visiting a legitimate website that triggered a redirect chain through multiple hops ending at a suspicious external domain. A JavaScript file was downloaded followed by a Windows executable. No antivirus alert was generated at the time, highlighting a detection gap. The executable filename mimics a legitimate system service.