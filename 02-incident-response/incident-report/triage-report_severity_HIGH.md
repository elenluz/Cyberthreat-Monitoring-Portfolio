# TRIAGE REPORT FORM

| Analyst Name | Elen Luz N. Gayondato |
|---|---|

---

| Date & Time | August 24, 2017 3:27 AM |
|---|---|
| Ticket ID | TKT-002 |
| Severity (Critical / High / Medium / Low) | HIGH |
| Kill Chain Phase | Delivery |
| MITRE ATT&CK Technique(s) | T1566.001, T1203 |
| Affected Host(s) | wrk-btun (billy.tun) |
| Source IP / Attacker IP | 104.47.37.62 (Microsoft EOP/O365) |

## Summary of Findings (what happened, how you found it, evidence observed)

A spearphishing email with a password-protected attachment (`invoice.zip`) was delivered to the environment. The user opened the document, triggering the execution of malicious code via MS Word. Indicators of suspicious behavior were observed on the affected host and in email headers.

## IOCs Identified (IPs, hashes, domains, filenames)

- Filename: `invoice.zip`, `invoice.doc` (Trojan)  
- Sender: `jsmith@urinalysis.com`

## Recommended Response Actions (containment, eradication, recovery)

### Containment
- Immediately isolate the affected workstation `wrk-btun` from the internal network to prevent lateral movement and further exfiltration.  
- Block the sender domain `urinalysis.com` at the enterprise email gateway and the perimeter firewall to prevent further delivery of malicious emails.  
- Terminate all active network sessions and RDP connections currently associated with the user account `billy.tun`.

### Eradication
- Perform a comprehensive scan of the endpoint `wrk-btun` to identify and remove the malicious `invoice.doc` file and any residual malware artifacts.  
- Disable the compromised user account `billy.tun` and mandate an immediate password reset, followed by revocation of all active Kerberos tickets to ensure no lingering access.  
- Search the environment for other hosts that may have received the same `invoice.zip` attachment and perform necessary clean-up actions on those systems.

### Recovery
- Re-image the `wrk-btun` workstation from a known-secure baseline to ensure no hidden persistence mechanisms remain.  
- Restore the user's access only after full credential rotation and verification of endpoint security status.  
- Monitor the environment for any anomalous traffic patterns related to the host or the compromised account for a period of 48–72 hours following the re-imaging.

## Lessons Learned / Detection Gap

Initial delivery bypassed AV/Mail Gateway due to password protection (encrypting the payload). Detection required behavioral monitoring of spawned processes from MS Word and improved inspection of password-protected attachments at mail gateways.