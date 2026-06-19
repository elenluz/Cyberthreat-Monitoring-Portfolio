# TRIAGE REPORT FORM

| Analyst Name | Elen Luz N. Gayondato |
|---|---|

---

| Date & Time | June 29, 2017, 10:51 AM |
|---|---|
| Ticket ID | TKT-009 |
| Severity (Critical / High / Medium / Low) | Medium |
| Kill Chain Phase | Exploitation |
| MITRE ATT&CK Technique(s) | T1110.001, T1078 |
| Affected Host(s) | 10.0.1.100 |
| Source IP / Attacker IP | 71.39.18.121 |

## Summary of Findings (what happened, how you found it, evidence observed)

A sophisticated, multi-stage attack was detected against the Frothly Domain Controller. The incident began with the compromise of host 10.0.1.100 (Patient Zero), which was used to perform automated lateral movement and brute-force reconnaissance (high-volume failed logon attempts / EventCode 4625) against the internal network. This reconnaissance culminated in a successful interactive compromise (Logon Type 10) of the Administrator account by an external threat actor originating from 71.39.18.121. Analysis of security event logs (EventCode 4624/4625) provided definitive evidence of a credential-stuffing campaign and subsequent unauthorized RDP access to domain resources.

## IOCs Identified (IPs, hashes, domains, filenames)

- External Attacker IP: 71.39.18.121  
- Internal Compromised Host (Patient Zero): 10.0.1.100  
- Affected Account: Administrator  
- Lateral Movement Sources: Multiple internal hosts including 10.0.2.109, 10.0.1.101, and 10.0.2.107

## Recommended Response Actions (containment, eradication, recovery)

### Containment
- Immediately isolate 10.0.1.100 from the network.  
- Block 71.39.18.121 at the perimeter firewall and any edge filtering appliances.  
- Disable the compromised Administrator account and revoke all active RDP sessions on the Domain Controller.

### Eradication
- Perform mandatory password resets for the Administrator account and for all accounts that logged into 10.0.1.100 within the last 48 hours.  
- Execute a KRBTGT password reset (twice) to invalidate existing Kerberos tickets.  
- Audit for newly created accounts, scheduled tasks, services, or other persistence mechanisms and remove any unauthorized artifacts.  
- Search and remediate lateralized hosts (10.0.2.109, 10.0.1.101, 10.0.2.107, etc.) that may have been used during the compromise.

### Recovery
- Re-image affected hosts from known-good baselines.  
- Re-establish Domain Controller security posture by strictly limiting RDP access (use VPN + MFA or a bastion/jump-host) and enforcing least privilege for administrative accounts.  
- Restore services only after credential rotation, ticket invalidation, and verification that no persistence remains.  
- Continue enhanced monitoring for anomalous logon activity and lateral movement indicators for an extended period post-recovery.

## Lessons Learned / Detection Gap

The environment lacked automated alerting for high-volume failed logon attempts (EventCode 4625), allowing the brute-force phase to persist for ~45 minutes undetected. The Domain Controller was directly exposed to the internet via RDP. Detection/mitigation gaps include the need for threshold-based alerts for logon failures, adoption of a 'Zero Trust' approach for administrative RDP access (VPN + MFA + jump-box), and proactive blocking/throttling of suspicious inbound RDP connections.