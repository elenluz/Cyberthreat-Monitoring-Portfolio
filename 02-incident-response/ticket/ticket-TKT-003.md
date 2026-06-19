# TKT-003 | SQL Injection Attempt on E-Commerce Web Application

| Ticket ID | TKT-003 |
|---|---|
| Date & Time | August 11, 2017, 2:40 PM |
| Severity (Critical / High / Medium / Low) | CRITICAL |
| Kill Chain Phase | Exploitation |
| MITRE ATT&CK Technique(s) | T1190 · T1059.001 |
| Incident Report | [triage-report_SQLi_UnionSelect_Attack.md](../02-incident-response/triage-report_SQLi_UnionSelect_Attack.md) |

## SCENARIO

The web application firewall and Suricata IDS both logged alerts against the Frothly web server. External IP 45.77.65.211 was observed sending crafted HTTP requests containing SQL injection strings (UNION SELECT) targeting the web application. Two requests returned unusually large HTTP responses, suggesting the backend database may have returned unauthorized records and potential data exfiltration.