<!-- Replace bracketed placeholders with your real details before publishing. -->

# CASE-001 · SIEM Threat Detection & Dashboard Build

`Status: Documented` · `Category: Detection & Monitoring` · `Tools: Splunk Enterprise, BOTSv2 Dataset, SPL`

## Overview

This case covers standing up a working SIEM environment and learning to turn raw security logs into actionable monitoring. The lab uses **BOTSv2 (Boss of the SOC v2)** — a publicly available dataset built by Splunk for SOC training, containing logs from a simulated breach against a fictional brewing company ("Frothly"). It includes a mix of Windows event logs, network/wire data, and endpoint telemetry, which makes it realistic enough to practice real detection workflows on.

## Lab Environment

| Component | Detail |
|---|---|
| SIEM Platform | Splunk Enterprise [version] |
| Dataset | BOTSv2 |
| Indexes Used | `[index names, e.g., botsv2]` |
| Key Sourcetypes | `[e.g., WinEventLog:Security, stream:http, suricata]` |
| Deployment | [Single instance on local VM / Docker / etc.] |

## Methodology

1. **Ingest & orient** — loaded the BOTSv2 dataset into Splunk and ran broad searches (`index=botsv2`) to understand what sourcetypes and fields were available before writing any targeted queries.
2. **Hunt with SPL** — wrote searches to surface indicators of compromise across the simulated environment.
3. **Build dashboards** — translated the most useful searches into permanent dashboard panels: time charts for activity volume, tables for top talkers/users, and single-value panels for at-a-glance health checks.
4. **Rehearse the demo** — practiced presenting the dashboard live, narrating what each panel shows and why it matters to someone without a security background.

## Key SPL Queries

```spl
# Detects the top 10 most active source IP addresses generating web traffic.
index=botsv2 sourcetype=stream:http | top limit=10 src_ip

# Detects the frequency of different HTTP status codes to monitor web server responses.
index=botsv2 sourcetype=stream:http | stats count by status

# Detects the volume of web traffic over time, grouped into 1-hour intervals.
index=botsv2 sourcetype=stream:http | timechart count span=1h

# Detects the top 10 most frequent successful Windows login accounts.
index=botsv2 sourcetype=WinEventLog:Security EventCode=4624 | stats count by Account_Name | sort -count | head 10

# Detects the total number of events indexed in the dataset.
index=botsv2 | stats count
```

## Findings

- [Finding 1 — what you found and why it mattered, e.g., "Identified a spike in failed authentication attempts against a single account over a 10-minute window."]
- [Finding 2]
- [Finding 3]

## Dashboard

![Dashboard screenshot](./screenshots/dashboard-overview.png)
*[One-line caption: what this panel shows and how to read it.]*

## Skills Demonstrated

- SPL query writing (search, stats, timechart, eval)
- Log correlation across multiple sourcetypes
- Dashboard and visualization design
- Translating technical findings for a non-technical audience

## Reflection

[1–3 sentences: what was the hardest part of this case, and what would you do differently with more time? This section shows judgment, not just task completion — it's often what recruiters remember.]
