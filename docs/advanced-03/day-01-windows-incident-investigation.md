# Advanced 3 — Day 1: Windows Incident Investigation + Timeline Reconstruction

## Objective

Perform a controlled Windows incident simulation on `soc-windows-01`, validate Wazuh and Sysmon detections, correlate related process activity, reconstruct an analyst timeline, identify detection gaps, and classify the incident.

## Environment

| System | Role | Internal IP |
| --- | --- | --- |
| `soc-ubuntu` | Wazuh Manager | `192.168.56.20` |
| `soc-linux-02` | Linux endpoint / Agent `001` | `192.168.56.30` |
| `soc-windows-01` | Windows 11 endpoint / Agent `002` | `192.168.56.40` |

## Pre-Incident Validation

The Wazuh Manager and both endpoint agents were confirmed active.

On `soc-windows-01`, the following services were verified:

```powershell
Get-Service WazuhSvc, Sysmon64
```

Result:

```text
Running  Sysmon64  Sysmon64
Running  WazuhSvc  Wazuh
```

An incident start marker was created before generating controlled activity.

## Controlled Incident Activity

### Suspicious PowerShell Execution

The following controlled command was executed:

```powershell
powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "Write-Output 'A3-DAY1 simulated suspicious PowerShell execution'"
```

Sysmon captured the process creation and Wazuh generated:

```text
Rule: 100600
Level: 10
Description: Custom Windows detection: PowerShell executed with ExecutionPolicy Bypass.
```

MITRE ATT&CK:

```text
T1059.001 — Command and Scripting Interpreter: PowerShell
```

## Discovery Activity

Follow-on Windows discovery commands were executed:

```powershell
net user
net localgroup administrators
```

The investigation identified:

```text
net.exe user
net1.exe user
net.exe localgroup administrators
net1.exe localgroup administrators
```

Wazuh generated:

```text
Rule: 92033
Level: 3
Description: Discovery activity spawned via powershell execution
```

and:

```text
Rule: 92031
Level: 3
Description: Discovery activity executed
```

## Process Correlation

The process relationships were reconstructed as:

```text
powershell.exe
├── net.exe user
│   └── net1.exe user
└── net.exe localgroup administrators
    └── net1.exe localgroup administrators
```

This linked the discovery activity to the PowerShell session rather than treating the alerts as unrelated events.

## Investigation Timeline

| Approx. UTC Time | Detection | Activity |
| --- | --- | --- |
| 18:07 | Rule `100600`, Level 10 | PowerShell executed with `-ExecutionPolicy Bypass` |
| 18:09 | Rule `92033`, Level 3 | `net.exe user` spawned from PowerShell |
| 18:09 | Rule `92031`, Level 3 | `net1.exe user` |
| 18:09 | Rule `92033`, Level 3 | `net.exe localgroup administrators` |
| 18:09 | Rule `92031`, Level 3 | `net1.exe localgroup administrators` |

The Windows incident end marker was:

```text
A3-DAY1 INCIDENT END 2026-08-27 02:22:54
```

Windows local time and Wazuh/Sysmon UTC timestamps were considered during timeline reconstruction.

## Detection Analysis

Primary detection:

```text
Rule ID: 100600
Level: 10
Endpoint: soc-windows-01
User: SOC-WINDOWS-01\soc-windows-01
Sysmon Event ID: 1
Process: powershell.exe
```

Follow-on discovery detections:

```text
Rule 92033
Rule 92031
```

## MITRE ATT&CK Mapping

| Activity | Technique |
| --- | --- |
| PowerShell ExecutionPolicy Bypass | `T1059.001` — PowerShell |
| Account discovery | `T1087` — Account Discovery |
| Local group discovery | `T1069.001` — Permission Groups Discovery: Local Groups |

## Detection Gaps

Additional controlled activity such as:

```text
ipconfig /all
systeminfo
incident-note.txt creation
```

did not produce the specific alert evidence expected in `alerts.log`.

This demonstrated the difference between:

```text
Activity occurs
      ↓
Telemetry generated
      ↓
Telemetry ingested
      ↓
Detection rule matches
      ↓
Alert generated
```

A telemetry source may exist without a matching alert rule.

## Analyst Assessment

```text
Classification:
TRUE POSITIVE — CONTROLLED LAB SIMULATION

Affected endpoint:
soc-windows-01

Affected user:
SOC-WINDOWS-01\soc-windows-01

Primary detection:
Rule 100600 — Level 10

Follow-on activity:
Account and privileged-group discovery
```

The activity was intentionally generated in the isolated SOC lab, so no real compromise occurred.


## Result

**PASS — A controlled Windows incident was generated, detected, correlated into a process chain, reconstructed as an analyst timeline, mapped to MITRE ATT&CK, and classified as a true-positive controlled lab simulation.**

## Advanced 3 Status

```text
✅ A3-Day 1 — Windows incident investigation + timeline reconstruction
⬜ A3-Day 2 — Incident triage + evidence enrichment
⬜ A3-Day 3 — Investigation report + lessons learned
```

## Next

**A3-Day 2 — Incident triage + evidence enrichment.**
