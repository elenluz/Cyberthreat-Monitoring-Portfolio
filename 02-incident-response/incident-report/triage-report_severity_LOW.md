# TRIAGE REPORT FORM

| Analyst Name | Elen Luz N. Gayondato |
|---|---|

---

| Date & Time | August 15, 2017, 11:35 AM |
|---|---|
| Ticket ID | TKT-010 |
| Severity (Critical / High / Medium / Low) | LOW |
| Kill Chain Phase | Delivery |
| MITRE ATT&CK Technique(s) | T1189, T1204.001 |
| Affected Host(s) | 10.0.2.109 |
| Source IP / Attacker IP | 45.77.65.211 (Attacker-controlled server) |

## Summary of Findings (what happened, how you found it, evidence observed)

A Frothly workstation (10.0.2.109) was compromised via a drive-by browser exploit. The workstation visited http://www.brewertalk.com which redirected the browser through a malicious chain that delivered an XSS payload. The payload executed a script that exfiltrated the user's authentication/session cookie and other document.cookie data to the attacker-controlled server at 45.77.65.211. Subsequent traffic and application logs indicate the attacker hijacked the authenticated session and performed unauthorized administrative actions, including creation of a rogue account named `klagerfield`.

## IOCs Identified (IPs, hashes, domains, filenames)

- Attacker IP: 45.77.65.211  
- Malicious/Compromised Domain: www.brewertalk.com  
- Rogue username/account: klagerfield

## Recommended Response Actions (containment, eradication, recovery)

### Containment
- Block all traffic to and from 45.77.65.211 at the perimeter firewall and via DNS filtering.  
- If possible, block or place `www.brewertalk.com` on a denylist until the site is confirmed clean.

### Eradication
- Disable the unauthorized account `klagerfield` and invalidate all active sessions and authentication cookies associated with the compromised user(s).  
- Revoke any elevated privileges granted to the rogue account and remove any artifacts (scheduled tasks, web application changes, uploaded files) created by the attacker.  
- Scan the affected host (10.0.2.109) for indicators of browser-based or fileless persistence; remove any malicious scripts or injected code.

### Recovery
- Remediate the XSS vulnerability in the affected web application(s) by implementing proper input validation, server-side output encoding, and contextual escaping.  
- Apply Content Security Policy (CSP) and other browser hardening controls where appropriate.  
- Re-image the compromised workstation if evidence of deeper compromise or persistence is found; otherwise perform a full forensic imaging and cleanup.  
- Rotate affected user credentials and monitor for any replay or re-use of exfiltrated tokens.  
- Continue heightened monitoring for any follow-up access from the attacker infrastructure.

## Lessons Learned / Detection Gap

Detection Gap: Lack of browser/endpoint telemetry for suspicious script execution and reliance on signature-based AV allowed the XSS-driven session hijack to proceed undetected.  

Lessons Learned: Implement DNS filtering and web proxying to block known malicious domains, enable browser isolation or content-disarm-and-reconstruction for high-risk browsing, enforce robust input/output encoding in web apps, and improve session token protections (secure, HttpOnly cookies, short session TTLs, and anomalous session detection). Enhance logging to capture browser-originated exfiltration attempts and correlate web/app logs with proxy/DNS telemetry.