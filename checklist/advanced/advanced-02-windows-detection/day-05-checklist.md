# Advanced 2 — Day 5 Checklist

## Pre-Deployment
- [x] Reviewed existing Wazuh custom rules
- [x] Created backup of `local_rules.xml`
- [x] Validated Wazuh rule configuration
- [x] Confirmed `wazuh-analysisd -t` returned exit code `0`

## Custom Detection Development
- [x] Created custom Windows PowerShell detection rule
- [x] Created Rule `100600`
- [x] Configured PowerShell `ExecutionPolicy Bypass` detection
- [x] Added MITRE mapping `T1059.001`
- [x] Created custom Windows account discovery detection rule
- [x] Created Rule `100601`
- [x] Configured `net.exe` account discovery detection
- [x] Added MITRE mapping `T1087`

## Detection Validation
- [x] Restarted Wazuh manager after rule changes
- [x] Generated controlled PowerShell activity
- [x] Confirmed Rule `100600`
- [x] Confirmed level 10 PowerShell detection
- [x] Generated controlled `net user` activity
- [x] Generated controlled `net localgroup` activity
- [x] Confirmed Rule `100601`
- [x] Confirmed level 10 account discovery detection

## SOC Evidence Collection
- [x] Confirmed Agent `002`
- [x] Confirmed provider `Microsoft-Windows-Sysmon`
- [x] Confirmed Sysmon Event ID `1`
- [x] Confirmed custom Wazuh alerts
- [x] Captured PowerShell detection evidence
- [x] Captured net.exe detection evidence

## Result

- [x] **PASS — A2-Day 5 COMPLETE**

## Next

- [ ] **Advanced 3 — Threat simulation, incident investigation, and detection improvement**
