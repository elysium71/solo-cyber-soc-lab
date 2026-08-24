# Advanced 2 — Day 3: Sysmon Deployment + Wazuh Integration

## Objective
Deploy Microsoft Sysmon on `soc-windows-01`, apply a security-focused configuration, configure Wazuh to collect Sysmon telemetry, and validate the events centrally.

## Environment

| System | Role | Internal IP |
| --- | --- | --- |
| `soc-ubuntu` | Wazuh Manager | `192.168.56.20` |
| `soc-linux-02` | Linux endpoint / Agent `001` | `192.168.56.30` |
| `soc-windows-01` | Windows 11 endpoint / Agent `002` | `192.168.56.40` |

## Pre-Deployment Validation
Sysmon was not previously installed, while `WazuhSvc` was confirmed Running.

## Sysmon Deployment
Sysmon was downloaded to `C:\SOC-LAB\Sysmon` and the SwiftOnSecurity Sysmon configuration was saved as `sysmonconfig.xml`.

Sysmon was installed with:

```powershell
& "C:\SOC-LAB\Sysmon\Sysmon64.exe" -accepteula -i "C:\SOC-LAB\Sysmon\sysmonconfig.xml"
```

Installation validation included:

```text
Configuration file validated.
Sysmon64 installed.
SysmonDrv installed.
SysmonDrv started.
Sysmon64 started.
```

The service was verified with:

```powershell
Get-Service Sysmon64
```

Result:

```text
Running  Sysmon64  Sysmon64
```

## Local Event Validation

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5 |
Select-Object TimeCreated, Id, ProviderName
```

Sysmon generated local events including IDs `1`, `4`, and `16`.

## Wazuh Integration
The Windows Wazuh configuration was backed up and the following block was added before `</ossec_config>`:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

The Wazuh agent service was restarted.

## Central Validation
On `soc-ubuntu`, Wazuh confirmed Sysmon events from `soc-windows-01`.

A validated event contained:

```text
Provider: Microsoft-Windows-Sysmon
Channel: Microsoft-Windows-Sysmon/Operational
Event ID: 11
Agent: soc-windows-01
```

Wazuh Rule `92217` generated a level 6 detection for the Sysmon file-creation event.

## Dashboard Validation
The Wazuh dashboard displayed the event with:

```text
agent.id:                     002
agent.name:                   soc-windows-01
data.win.system.providerName: Microsoft-Windows-Sysmon
data.win.system.channel:      Microsoft-Windows-Sysmon/Operational
data.win.system.eventID:      11
data.win.eventdata.user:      NT AUTHORITY\SYSTEM
```

The event also exposed process image and target filename information.


## Result
**PASS — Sysmon was deployed on `soc-windows-01`, local telemetry was generated, Wazuh was configured to collect the Sysmon Operational channel, and centralized Sysmon telemetry was validated.**

## Advanced 2 Status

```text
✅ A2-Day 1 — Build + network Windows endpoint
✅ A2-Day 2 — Wazuh Windows agent + telemetry
✅ A2-Day 3 — Sysmon deployment + Wazuh integration
⬜ A2-Day 4 — Windows detection validation
```

## Next
**A2-Day 4 — Generate controlled Windows activity and validate Windows/Sysmon detections in Wazuh.**
