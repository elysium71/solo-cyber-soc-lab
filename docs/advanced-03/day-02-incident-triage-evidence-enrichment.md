# Advanced 3 — Day 2: Incident Triage + Evidence Enrichment

## Objective
Perform analyst-style triage and evidence enrichment on the Windows incident identified during Advanced 3 Day 1. Recover the original Wazuh alert, extract endpoint and process context, correlate related discovery activity, validate MITRE ATT&CK metadata, and produce a structured incident assessment.

## Environment

| Component | Details |
| --- | --- |
| Wazuh Manager | `soc-ubuntu` |
| Primary Endpoint | `soc-windows-01` |
| Wazuh Agent ID | `002` |
| Endpoint IP | `192.168.56.40` |
| Telemetry | Microsoft Sysmon |
| Primary Rule | `100600` |
| Related Rules | `92033`, `92031` |
| Classification | True Positive — Controlled Lab Simulation |

## Pre-Investigation Validation
Wazuh health was validated before investigation. Agents `000` (`soc-ubuntu`), `001` (`soc-linux-02`), and `002` (`soc-windows-01`) were active, and the Wazuh Manager service was active.

## Archived Alert Recovery
The Day 1 alert was no longer in the current `alerts.json`. The original Rule `100600` event was recovered from:

```text
/var/ossec/logs/alerts/2026/Aug/ossec-alerts-26.json.gz
```

## Primary Alert Triage

| Field | Value |
| --- | --- |
| Rule ID | `100600` |
| Level | `10` |
| Description | Custom Windows detection: PowerShell executed with ExecutionPolicy Bypass |
| Endpoint | `soc-windows-01` |
| Agent ID | `002` |
| IP | `192.168.56.40` |
| Provider | Microsoft-Windows-Sysmon |
| Event ID | `1` — Process Create |
| Process | `powershell.exe` |
| PID | `10160` |
| User | `SOC-WINDOWS-01\\soc-windows-01` |
| Integrity | `High` |
| Parent Process | `powershell.exe` |
| Parent PID | `4088` |
| ATT&CK | `T1059.001` — PowerShell |
| Tactic | Execution |

The command line contained `-ExecutionPolicy Bypass`, matching the behavior targeted by custom Rule `100600`.

## Related Alert Correlation
Related discovery activity followed shortly afterward:

```text
18:09:05 UTC | Rule 92033 | net.exe  user
18:09:05 UTC | Rule 92031 | net1.exe user
18:09:06 UTC | Rule 92033 | net.exe  localgroup administrators
18:09:06 UTC | Rule 92031 | net1.exe localgroup administrators
```

The events occurred on the same Windows endpoint and under the same user context.

## Process and User Enrichment

```text
powershell.exe (PID 4088)
├── net.exe (PID 11024)
│   └── net1.exe (PID 4540)
└── net.exe (PID 1256)
    └── net1.exe (PID 1052)
```

All four discovery processes ran as `SOC-WINDOWS-01\soc-windows-01` with High integrity.

The Rule `100600` event recorded a separate PowerShell process with PID `10160`. The discovery processes had parent PID `4088`, corresponding to the parent PowerShell session. The evidence therefore supports correlation through the shared PowerShell session and user context without claiming PID `10160` directly spawned the discovery commands.

## MITRE ATT&CK Enrichment
ATT&CK metadata was taken directly from the Wazuh alerts:

- Rule `100600`: `T1059.001` — PowerShell; tactic: Execution.
- Rule `92033`: `T1087` — Account Discovery and `T1059.001` — PowerShell; tactics: Discovery and Execution.
- Rule `92031`: `T1087` — Account Discovery; tactic: Discovery.

No additional technique was assigned beyond the alert metadata observed in Wazuh.

## Incident Case Summary
**Case ID:** `A3-DAY2-CASE-001`  
**Classification:** TRUE POSITIVE — CONTROLLED LAB SIMULATION  
**Primary detection:** Rule `100600`, Level `10`  
**Related activity:** Rules `92033` and `92031`  
**Endpoint:** `soc-windows-01` (`192.168.56.40`)  
**User:** `SOC-WINDOWS-01\soc-windows-01`  
**Status:** CLOSED — CONTROLLED TEST

Suspicious PowerShell execution using `ExecutionPolicy Bypass` was followed by account discovery commands on the same endpoint and user context. Wazuh and Sysmon telemetry provided process, parent-process, user, severity, and ATT&CK information sufficient to correlate the activity into one analyst case. The activity was intentionally generated as part of the authorized Mini SOC Lab. No evidence identified during this investigation indicates a real compromise.

## Analyst Findings
1. Archived Wazuh data allowed the original incident to be recovered after alert rotation.
2. Custom Rule `100600` provided a Level 10 triage starting point.
3. Sysmon Event ID 1 supplied detailed process and parent-process context.
4. Rules `92033` and `92031` provided follow-on discovery detections.
5. ATT&CK metadata contextualized the activity across Execution and Discovery.
6. PID/parent-PID evidence prevented overstating the relationship between the Rule `100600` process and later discovery commands.



## Result
Advanced 3 Day 2 successfully converted the Day 1 Windows detection into a structured SOC investigation case. The alert was recovered from archived Wazuh data, enriched with Sysmon telemetry, correlated with follow-on discovery activity, mapped using Wazuh ATT&CK metadata, and closed as an authorized controlled simulation.



## Next
**Advanced 3 — Day 3: Investigation Report + Lessons Learned**
