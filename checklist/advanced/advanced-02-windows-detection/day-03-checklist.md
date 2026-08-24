# Advanced 2 — Day 3 Checklist

## Pre-Deployment
- [x] Confirmed Sysmon was not previously installed
- [x] Confirmed `WazuhSvc` was Running

## Sysmon Deployment
- [x] Created `C:\SOC-LAB\Sysmon`
- [x] Downloaded and extracted Sysmon
- [x] Downloaded Sysmon configuration
- [x] Validated configuration
- [x] Installed `Sysmon64`
- [x] Started Sysmon driver and service
- [x] Verified `Sysmon64` is Running

## Local Telemetry
- [x] Confirmed `Microsoft-Windows-Sysmon/Operational`
- [x] Confirmed Sysmon events were generated
- [x] Observed Event ID `1`
- [x] Observed additional Sysmon events

## Wazuh Integration
- [x] Backed up `ossec.conf`
- [x] Added Sysmon EventChannel collection
- [x] Restarted Wazuh agent
- [x] Verified Sysmon telemetry reached the manager

## Detection Validation
- [x] Confirmed Agent `002`
- [x] Confirmed provider `Microsoft-Windows-Sysmon`
- [x] Confirmed Sysmon Operational channel
- [x] Confirmed Event ID `11`
- [x] Confirmed Wazuh Rule `92217`
- [x] Confirmed level 6 detection
- [x] Opened the Sysmon event in Wazuh dashboard

## Result
- [x] **PASS — A2-Day 3 COMPLETE**

## Next
- [ ] **A2-Day 4 — Windows detection validation**
