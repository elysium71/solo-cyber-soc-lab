# Advanced 3 — Day 3: Investigation Report + Lessons Learned

## Objective
Complete Advanced 3 by consolidating the Windows incident into a final timeline, evaluating detection effectiveness, documenting limitations, and recording lessons learned and SOC recommendations.

## Environment
| Component | Details |
| --- | --- |
| Wazuh Manager | `soc-ubuntu` |
| Endpoint | `soc-windows-01` |
| Agent | `002` |
| IP | `192.168.56.40` |
| Telemetry | Microsoft Sysmon |
| Primary Rule | `100600` — Level 10 |
| Related Rules | `92033`, `92031` |
| Case | `A3-DAY2-CASE-001` |
| Classification | True Positive — Controlled Lab Simulation |

## Evidence Validation
Rule `100600` was revalidated from `/var/ossec/logs/alerts/2026/Aug/ossec-alerts-26.json.gz`. The event confirmed PowerShell execution using `-ExecutionPolicy Bypass` on `soc-windows-01`, user `SOC-WINDOWS-01\soc-windows-01`, with Wazuh ATT&CK metadata `T1059.001`.

## Final Incident Timeline
```text
18:07:22 UTC | 100600 | L10 | PowerShell -ExecutionPolicy Bypass
        |
        | ~1 minute 42 seconds
        v
18:09:05 UTC | 92033 | L3 | net.exe user
18:09:05 UTC | 92031 | L3 | net1.exe user
18:09:06 UTC | 92033 | L3 | net.exe localgroup administrators
18:09:06 UTC | 92031 | L3 | net1.exe localgroup administrators
```

## Process Correlation
```text
powershell.exe (PID 4088)
├── net.exe (PID 11024)
│   └── net1.exe (PID 4540)
└── net.exe (PID 1256)
    └── net1.exe (PID 1052)
```

The primary Rule `100600` event used PowerShell PID `10160`. The discovery processes referenced PID `4088` as their parent. The activity is therefore correlated through endpoint, user, timing and PowerShell session context without incorrectly claiming PID `10160` directly spawned the discovery commands.

## MITRE ATT&CK Context
- Rule `100600`: `T1059.001` — PowerShell; Execution.
- Rule `92033`: `T1087` — Account Discovery and `T1059.001` — PowerShell; Discovery and Execution.
- Rule `92031`: `T1087` — Account Discovery; Discovery.

These mappings are based on the metadata contained in the observed Wazuh alerts.

## Detection Effectiveness

### Strengths
- Sysmon captured detailed process creation telemetry.
- Command-line, user, integrity and parent-process context were available.
- Rule `100600` provided a Level 10 triage starting point.
- Wazuh detected related discovery activity.
- ATT&CK metadata supported enrichment.
- Archived alerts allowed investigation after log rotation.

### Gaps / Limitations
- Some controlled Day 1 activity did not produce the specific alert evidence sought.
- Discovery alerts were Level 3 and required analyst context.
- Multiple alerts required manual correlation.
- Historical investigation required archived logs after rotation.
- Rule `100600` detects a specific PowerShell pattern, not all malicious PowerShell behavior.

## Lessons Learned
1. A single alert does not describe the full incident.
2. Sysmon materially improves Windows investigations.
3. Severity provides a useful triage signal.
4. ATT&CK mapping improves investigation context.
5. Historical log retention is operationally important.
6. Process correlation should be based on observed PID/parent-PID evidence.

## SOC Recommendations
- Correlate related endpoint alerts into incident cases.
- Retain sufficient searchable historical alert data.
- Continue tuning high-value Wazuh rules.
- Maintain Sysmon visibility on Windows endpoints.
- Consider contextual escalation when discovery follows suspicious execution.
- Preserve ATT&CK mappings in detection documentation.
- Validate detections through authorized controlled simulations.

## Analyst Assessment
The monitoring stack detected the primary suspicious PowerShell behavior and subsequent discovery activity. A useful incident assessment required correlation across process telemetry, user context, timestamps, severity, process ancestry and ATT&CK metadata.

**Classification:** TRUE POSITIVE — CONTROLLED LAB SIMULATION  
**Status:** CLOSED — CONTROLLED TEST

No evidence identified during the investigation indicated a real compromise outside the intentionally generated lab activity.


## Result
Advanced 3 demonstrated an end-to-end SOC investigation workflow: Detection → Triage → Evidence Enrichment → Correlation → ATT&CK Context → Assessment → Case Closure → Lessons Learned.

## Advanced 3 Status
- [x] Day 1 — Windows incident investigation + timeline reconstruction
- [x] Day 2 — Incident triage + evidence enrichment
- [x] Day 3 — Investigation report + lessons learned

**Advanced 3: 3 / 3 COMPLETE**

## Next
**Advanced 4 — Wazuh Active Response**
