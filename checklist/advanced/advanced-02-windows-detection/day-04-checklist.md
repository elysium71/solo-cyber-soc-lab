# Advanced 2 — Day 4 Checklist

## Pre-Validation
- [x] Confirmed `Sysmon64` was Running
- [x] Confirmed `WazuhSvc` was Running
- [x] Confirmed Windows endpoint `soc-windows-01` was connected

## Controlled Activity Generation
- [x] Generated account discovery activity
- [x] Tested `net user`
- [x] Tested `net localgroup`
- [x] Generated PowerShell activity
- [x] Generated controlled process creation events

## Detection Validation
- [x] Confirmed Sysmon Event ID `1` process creation telemetry
- [x] Confirmed Agent `002`
- [x] Confirmed provider `Microsoft-Windows-Sysmon`
- [x] Confirmed Sysmon Operational channel
- [x] Confirmed Wazuh Rule `92039`
- [x] Confirmed account discovery detection
- [x] Confirmed Wazuh Rule `92027`
- [x] Confirmed PowerShell detection
- [x] Confirmed Wazuh dashboard visibility

## MITRE ATT&CK Mapping
- [x] Mapped Account Discovery activity to `T1087`
- [x] Mapped PowerShell activity to `T1059.001`

## Evidence Collection
- [x] Captured PowerShell detection screenshot
- [x] Captured account discovery detection screenshot
- [x] Documented detection flow from Windows activity to Wazuh alert

## Result
- [x] **PASS — A2-Day 4 COMPLETE**

## Next
- [ ] **A2-Day 5 — Custom detection engineering**