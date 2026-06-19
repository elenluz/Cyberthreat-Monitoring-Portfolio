<!-- Replace bracketed placeholders with your real details before publishing. -->

# CASE-002 · Incident Triage, Ticketing & Reporting

`Status: Documented` · `Category: Incident Response` · `Tools: [Ticketing platform], Incident Report Writing`

## Overview

This case covers the part of SOC work that happens *after* detection: deciding what matters, tracking it through to resolution, and writing it up clearly enough that another analyst could pick up the case cold. Findings from the [CASE-001 Splunk dashboard work](../01-splunk-siem-monitoring) fed directly into this workflow.

## Workflow

```
Alert / Finding  →  Ticket Logged  →  Triage  →  Investigation  →  Report  →  Close
```

1. **Log it** — every alert or finding worth tracking was entered into [ticketing platform] as a ticket, rather than left as a one-off search result.
2. **Triage it** — each ticket was scored for priority before deep investigation, using the criteria below.
3. **Investigate** — pulled supporting evidence (Splunk searches, packet captures, honeypot logs) to confirm or rule out the ticket.
4. **Report it** — higher-priority tickets were written up using the [incident report template](./incident-report-template.md).
5. **Close it** — ticket updated with final status and a link to the report.

## Triage / Priority Criteria

| Priority | Criteria | Sample Ticket | Sample Incident Report |
|---|---|---|---|
| **P1 – Critical** | Active compromise, confirmed data exposure | [TKT-003](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/ticket/ticket-TKT-003.md) | [triage-report_severity_CRITICAL.md](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/incident-report/triage-report_severity_CRITICAL.md) |
| **P2 – High** | Strong indicator of compromise, no confirmed impact yet | [TKT-002](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/ticket/ticket-TKT-002.md) | [triage-report_severity_HIGH.md](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/incident-report/triage-report_severity_HIGH.md) |
| **P3 – Medium** | Suspicious but plausible benign explanation | [TKT-009](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/ticket/ticket-TKT-009.md) | [triage-report_severity_MEDIUM.md](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/incident-report/triage-report_severity_MEDIUM.md) |
| **P4 – Low** | Informational / noise, logged for trend tracking | [TKT-010](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/ticket/ticket-TKT-010.md) | [triage-report_severity_LOW.md](https://github.com/elenluz/Cyberthreat-Monitoring-Portfolio/blob/main/02-incident-response/incident-report/triage-report_severity_LOW.md) |

## Skills Demonstrated

- Alert triage and prioritization under a defined framework
- Ticketing/case management workflow
- Incident report writing (summary, timeline, IOCs, recommendations)
- Clear, audience-aware technical communication

## Reflection

Translating the technical findings into an actionable ticket required careful judgment to accurately assign the severity and prioritize the response. Going forward, I would like to integrate more automated threat intelligence lookups directly into the reporting workflow to speed up the triage process.
