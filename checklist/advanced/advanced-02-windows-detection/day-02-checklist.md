# Advanced 2 — Day 2 Checklist

## Wazuh Agent
- [x] Installed Wazuh Agent `4.13.1`
- [x] Configured manager `192.168.56.20`
- [x] Configured agent name `soc-windows-01`
- [x] Started `WazuhSvc`
- [x] Verified service is Running

## Enrollment
- [x] Enrolled Windows endpoint
- [x] Assigned Agent ID `002`
- [x] Verified Agent `002` is Active
- [x] Verified Agent `001` remained Active

## Telemetry
- [x] Generated controlled Windows Application event
- [x] Verified Windows event creation
- [x] Verified `soc-windows-01` telemetry on manager
- [x] Verified Windows `EventChannel` telemetry
- [x] Observed decoded Windows event

## Final Validation
- [x] Ran `agent_control -i 002`
- [x] Confirmed Microsoft Windows 11 Pro
- [x] Confirmed Wazuh `v4.13.1`
- [x] Confirmed Active status
- [x] Confirmed Syscheck completed

## Result
- [x] **PASS — A2-Day 2 COMPLETE**

## Next
- [ ] **A2-Day 3 — Sysmon deployment**
