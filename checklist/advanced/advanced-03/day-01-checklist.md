# Advanced 3 — Day 1 Checklist

## Pre-Incident Validation
- [x] Confirmed Wazuh Manager was active
- [x] Confirmed Agents `001` and `002` were active
- [x] Confirmed `WazuhSvc` was Running
- [x] Confirmed `Sysmon64` was Running
- [x] Created incident start marker

## Controlled Incident Activity
- [x] Generated controlled PowerShell ExecutionPolicy Bypass activity
- [x] Confirmed Sysmon Process Create telemetry
- [x] Generated account discovery activity
- [x] Generated privileged-group discovery activity
- [x] Created controlled incident artifact
- [x] Created incident end marker

## Detection Validation
- [x] Validated custom Rule `100600`
- [x] Confirmed Rule `100600` Level 10 detection
- [x] Confirmed `-ExecutionPolicy Bypass`
- [x] Confirmed Rules `92033` and `92031`
- [x] Confirmed endpoint and user

## Investigation and Correlation
- [x] Identified PowerShell parent process
- [x] Identified `net.exe user` and `net1.exe user`
- [x] Identified `net.exe localgroup administrators` and `net1.exe localgroup administrators`
- [x] Correlated process chain
- [x] Reconstructed incident timeline
- [x] Distinguished relevant events from background activity
- [x] Documented local/UTC timestamp difference

## Detection Engineering
- [x] Reviewed Rule `100600`
- [x] Confirmed Sysmon Event ID `1`
- [x] Confirmed PowerShell image condition
- [x] Confirmed ExecutionPolicy Bypass condition
- [x] Confirmed MITRE ATT&CK `T1059.001`
- [x] Identified detection gaps

## Analyst Assessment
- [x] Identified affected endpoint and user
- [x] Identified primary and follow-on detections
- [x] Classified `TRUE POSITIVE — CONTROLLED LAB SIMULATION`

## Evidence
- [x] Lab health
- [x] PowerShell Bypass detection
- [x] Privileged-group discovery
- [x] Investigation evidence
- [x] Incident end marker
- [x] Incident timeline
- [x] Rule `100600` configuration

## Result
- [x] **PASS — A3-Day 1 COMPLETE**

## Next
- [ ] **A3-Day 2 — Incident triage + evidence enrichment**
