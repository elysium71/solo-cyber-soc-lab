# Advanced 2 — Day 4: Windows Detection Validation

## Objective
Generate controlled Windows activity on `soc-windows-01`, validate Sysmon process telemetry, confirm Wazuh detections, and validate the detection workflow centrally.

## Environment

| System | Role | Internal IP |
| --- | --- | --- |
| `soc-ubuntu` | Wazuh Manager | `192.168.56.20` |
| `soc-linux-02` | Linux endpoint / Agent `001` | `192.168.56.30` |
| `soc-windows-01` | Windows 11 endpoint / Agent `002` | `192.168.56.40` |

## Pre-Validation
Sysmon and Wazuh services were confirmed running before generating detection activity.

The following services were verified:

```powershell
Get-Service WazuhSvc, Sysmon64
```

Result:

```text
Running  Sysmon64  Sysmon64
Running  WazuhSvc  Wazuh
```

## Controlled Activity Generation

Controlled Windows discovery and PowerShell activity were generated on `soc-windows-01`.

### Account Discovery Activity

Commands executed:

```powershell
net user

net localgroup
```

These commands were used to validate Windows account discovery detection.

### PowerShell Activity

Command executed:

```powershell
powershell.exe -NoProfile -Command "Get-ComputerInfo | Select-Object WindowsProductName,WindowsVersion"
```

The activity generated Sysmon Event ID `1` process creation telemetry.

## Detection Validation

On `soc-ubuntu`, Wazuh alerts were reviewed to confirm Windows detection capability.

Account discovery detection:

```text
Rule: 92039
Level: 3

A net.exe account discovery command was initiated
```

PowerShell detection:

```text
Rule: 92027
Level: 4

Powershell process spawned powershell instance
```

A validated event contained:

```text
Provider: Microsoft-Windows-Sysmon
Channel: Microsoft-Windows-Sysmon/Operational
Event ID: 1
Agent: soc-windows-01
```

## Dashboard Validation

The Wazuh dashboard displayed the detected activity with:

```text
agent.id:                     002
agent.name:                   soc-windows-01
data.win.system.providerName: Microsoft-Windows-Sysmon
data.win.system.channel:      Microsoft-Windows-Sysmon/Operational
data.win.system.eventID:      1
data.win.eventdata.image:     powershell.exe / net.exe
```

The event information included process image, command line, user, and parent process details.

## MITRE ATT&CK Mapping

Account discovery activity:

```text
T1087 — Account Discovery
```

PowerShell activity:

```text
T1059.001 — Command and Scripting Interpreter: PowerShell
```

## Detection Flow

```text
Windows Activity
        |
        v
Sysmon Event ID 1
        |
        v
Wazuh Agent 002
        |
        v
Wazuh Detection Rule
        |
        v
SOC Investigation
```

## Result

**PASS — Controlled Windows activity was generated, Sysmon telemetry was collected, Wazuh detections were validated, and Windows detection capability was confirmed.**

## Advanced 2 Status

```text
✅ A2-Day 1 — Build + network Windows endpoint
✅ A2-Day 2 — Wazuh Windows agent + telemetry
✅ A2-Day 3 — Sysmon deployment + Wazuh integration
✅ A2-Day 4 — Windows detection validation
⬜ A2-Day 5 — Custom detection engineering
```

## Next

**A2-Day 5 — Create custom Wazuh detection rules and validate detection engineering workflows.**
