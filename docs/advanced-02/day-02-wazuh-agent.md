# Advanced 2 — Day 2: Wazuh Windows Agent + Telemetry

## Objective
Install Wazuh Agent 4.13.1 on `soc-windows-01`, enroll it with `soc-ubuntu`, and validate Windows telemetry.

## Installation
In Administrator PowerShell:
```powershell
Invoke-WebRequest `
  -Uri "https://packages.wazuh.com/4.x/windows/wazuh-agent-4.13.1-1.msi" `
  -OutFile "$env:TEMP\wazuh-agent.msi"

Start-Process msiexec.exe -Wait -ArgumentList `
'/i', "$env:TEMP\wazuh-agent.msi", `
'/q', `
'WAZUH_MANAGER=192.168.56.20', `
'WAZUH_AGENT_NAME=soc-windows-01'
```

Start and verify:
```powershell
Start-Service WazuhSvc
Get-Service WazuhSvc
```

## Manager Validation
On `soc-ubuntu`:
```bash
sudo /var/ossec/bin/agent_control -l
sudo /var/ossec/bin/agent_control -i 002
```

Validated:
```text
Agent ID:       002
Agent Name:     soc-windows-01
Status:         Active
Operating system: Microsoft Windows 11 Pro
Client version: Wazuh v4.13.1
```

## Windows Telemetry Test
A controlled Application event was generated:
```powershell
eventcreate /T INFORMATION /ID 100 /L APPLICATION /SO SOC-LAB /D "A2 Day 2 Wazuh Windows telemetry test"
```

Wazuh subsequently showed Windows EventChannel telemetry from `soc-windows-01`.

## Result
**PASS — Windows Agent `002` was enrolled and Windows telemetry reached Wazuh.**

## Next
**A2-Day 3 — Sysmon deployment + Wazuh integration.**
