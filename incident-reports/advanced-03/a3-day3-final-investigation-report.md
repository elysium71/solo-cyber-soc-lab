# Final Incident Investigation Report — Advanced 3

## Case Information
| Field | Value |
| --- | --- |
| Case ID | `A3-DAY2-CASE-001` |
| Endpoint | `soc-windows-01` |
| Agent | `002` |
| IP | `192.168.56.40` |
| Primary Detection | Rule `100600`, Level 10 |
| Related Detections | Rules `92033`, `92031` |
| Primary ATT&CK | `T1059.001` — PowerShell |
| Related ATT&CK | `T1087` — Account Discovery |
| Classification | True Positive — Controlled Lab Simulation |
| Status | Closed — Controlled Test |

## Executive Summary
A controlled Windows incident simulation generated suspicious PowerShell execution followed by account-discovery activity. Custom Wazuh Rule `100600` detected `-ExecutionPolicy Bypass` at Level 10. Approximately 1 minute 42 seconds later, Rules `92033` and `92031` detected `net.exe`/`net1.exe` discovery commands including `net user` and `net localgroup administrators`.

The incident was investigated using archived Wazuh alerts and Sysmon process telemetry. Endpoint, user, timestamp, command-line, integrity, PID, GUID, parent-process and ATT&CK context were used to reconstruct the activity. The case was classified as a true positive because the detections accurately represented intentionally generated lab activity. No evidence identified during the investigation indicated an actual compromise.

## Timeline
```text
18:07:22 | 100600 | L10 | PowerShell -ExecutionPolicy Bypass
   ↓ ~1m42s
18:09:05 | 92033 | L3  | net.exe user
18:09:05 | 92031 | L3  | net1.exe user
18:09:06 | 92033 | L3  | net.exe localgroup administrators
18:09:06 | 92031 | L3  | net1.exe localgroup administrators
```

## Process Analysis
```text
powershell.exe (PID 4088)
├── net.exe (PID 11024) → net1.exe (PID 4540)
└── net.exe (PID 1256)  → net1.exe (PID 1052)
```

Rule `100600` recorded a separate PowerShell PID `10160`. The discovery processes used PID `4088` as parent. The evidence supports correlation through endpoint, user, timing and PowerShell session context, but not a claim that PID `10160` directly spawned those discovery processes.

## ATT&CK Analysis
| Rule | Technique | Tactic |
| --- | --- | --- |
| `100600` | `T1059.001` — PowerShell | Execution |
| `92033` | `T1087` — Account Discovery; `T1059.001` — PowerShell | Discovery; Execution |
| `92031` | `T1087` — Account Discovery | Discovery |

Mappings above reflect the observed Wazuh alert metadata.

## Detection Effectiveness
Effective controls included the custom Level 10 PowerShell detection, detailed Sysmon process telemetry, follow-on Wazuh discovery alerts, process ancestry, ATT&CK enrichment, and archived alert retention.

Limitations included lower-severity discovery alerts, manual correlation requirements, some Day 1 activity without the specific expected alert evidence, dependence on archived logs after rotation, and the narrow behavior-specific scope of Rule `100600`.

## Scope and Impact
Activity was intentionally generated inside the authorized Mini SOC Lab and focused on `soc-windows-01`. No evidence identified during the investigation indicated spread to another endpoint, destructive activity, or genuine external compromise.

## Analyst Verdict
**TRUE POSITIVE — CONTROLLED LAB SIMULATION**

The detections accurately represented the authorized PowerShell and discovery activity.

## Recommendations
1. Correlate suspicious execution followed by discovery into incident cases.
2. Maintain searchable historical alert retention.
3. Continue testing and tuning custom Wazuh detections.
4. Preserve Sysmon process visibility.
5. Consider contextual escalation of discovery following high-severity execution.
6. Maintain ATT&CK metadata in detection documentation.
7. Continue authorized controlled validation.

## Lessons Learned
The exercise demonstrated that detection and investigation are complementary. A high-severity alert provided the starting point, but the incident story emerged only after correlating endpoint, user, process, parent process, timing, severity and ATT&CK information. It also demonstrated the operational value of historical log retention.

## Closure
**Case Status:** CLOSED — CONTROLLED TEST

Advanced 3 completed the workflow from initial detection through triage, evidence enrichment, timeline reconstruction, analyst assessment, lessons learned and formal case closure.
