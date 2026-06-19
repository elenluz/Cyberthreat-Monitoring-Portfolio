# TRIAGE REPORT FORM

| Analyst Name | Elen Luz N. Gayondato |
|---|---|

---

| Date & Time | August 11, 2017, 2:40 PM |
|---|---|
| Ticket ID | TKT-003 |
| Severity (Critical / High / Medium / Low) | CRITICAL |
| Kill Chain Phase | Exploitation |
| MITRE ATT&CK Technique(s) | T1190, T1059.001 |
| Affected Host(s) | 172.31.38.181 |
| Source IP / Attacker IP | 45.77.65.211 |

## Summary of Findings (what happened, how you found it, evidence observed)

The incident was initially detected when both the Web Application Firewall (WAF) and Suricata IDS triggered alerts regarding anomalous traffic directed at the internal Frothly web server (172.31.38.181). An investigation into network stream logs revealed that an external attacker (45.77.65.211) was actively targeting the application with in-band SQL injection attacks using crafted UNION SELECT strings. By hunting through the stream/http data, analysts were able to evaluate the exact HTTP status codes and response sizes associated with the attacker's activity to determine the scope and impact of the attack.

Log evidence confirmed a total of 778 injection attempts. While defensive systems successfully blocked 4 of these requests—returning an HTTP 403 Forbidden status—the vast majority (774 requests) returned an HTTP 200 OK, indicating the web server processed the malicious queries without crashing. Most critically, at least two of these successful requests generated unusually large HTTP response bodies exceeding 90 KB. An HTTP 200 OK coupled with an anomalously high bytes_out value deviating from baseline page loads provides strong evidence that the attacker forced the backend database to return a large volume of unauthorized records, confirming data exfiltration rather than only failed injection attempts.

## IOCs Identified (IPs, hashes, domains, filenames)

- Attacker IP: 45.77.65.211  
- Target Host: 172.31.38.181 (Frothly Web Server)  
- Attack Artifacts: HTTP requests containing malicious UNION SELECT SQL injection payloads

## Recommended Response Actions (containment, eradication, recovery)

### Containment
- Immediately block the attacker IP (45.77.65.211) at the perimeter firewall and WAF.  
- Suspend the affected web application or place it into maintenance mode to prevent further data exfiltration until the vulnerability is remediated.  
- Rotate credentials for the database service account used by the web application and restrict access from untrusted network locations.

### Eradication
- Identify the vulnerable input field/parameter in the web application code (the injection point) and remediate the SQL injection flaw by implementing parameterized queries (prepared statements) and strict input validation/whitelisting.  
- Review and patch any underlying web application frameworks or libraries if they contribute to the vulnerability (e.g., outdated DB drivers or ORM misuse).  
- Harden the WAF ruleset to detect and block the specific payload patterns observed.

### Recovery
- Perform a forensic audit of web and database logs to determine which tables and records were accessed and exactly what data was exfiltrated.  
- Notify stakeholders and follow applicable data breach notification procedures if sensitive data is confirmed exfiltrated.  
- Restore the web application from a known-good backup if integrity cannot be confirmed; restore service only after full verification and testing.  
- Continue heightened monitoring for follow-up or secondary activity and review compensating controls.

## Lessons Learned / Detection Gap

The WAF must be transitioned to active blocking mode for high-confidence SQLi signatures (e.g., UNION SELECT payloads). Enhanced logging and correlation between WAF, IDS, and application logs enabled rapid determination of exfiltration — continue refining alerting thresholds for anomalous response sizes (bytes_out) and suspicious response codes to detect data-returning SQLi attempts earlier.