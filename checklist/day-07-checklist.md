# Day 7 Checklist — Custom High-Value SSH Detection

## Preparation
- [x] Inspected existing `local_rules.xml`
- [x] Preserved existing Rule `100100`
- [x] Created `local_rules.xml.day7-backup`
- [x] Confirmed Kali source `192.168.56.10`
- [x] Confirmed Ubuntu/Wazuh target `192.168.56.20`

## Detection Engineering
- [x] Created Rule `100200`
- [x] Set severity to Level `12`
- [x] Used parent Rule `5716`
- [x] Matched target user `root`
- [x] Added MITRE ATT&CK `T1110`
- [x] Corrected XML group nesting
- [x] Corrected parent-rule logic
- [x] Corrected static username matching with `<user>root</user>`

## Validation and Live Test
- [x] Validated with `wazuh-logtest`
- [x] Confirmed Rule `100200` in Phase 3
- [x] Confirmed `Alert to be generated`
- [x] Restarted Wazuh Manager
- [x] Confirmed manager was active
- [x] Generated controlled failed root SSH login from Kali
- [x] Confirmed live Rule `100200` Level 12 alert
- [x] Confirmed source IP `192.168.56.10`
- [x] Confirmed target user `root`

## Threat Hunting / MITRE
- [x] Queried `rule.id:100200`
- [x] Verified `data.dstuser: root`
- [x] Verified `data.srcip: 192.168.56.10`
- [x] Verified `decoder.name: sshd`
- [x] Verified `rule.level: 12`
- [x] Verified `T1110`
- [x] Verified `Credential Access`
- [x] Verified `Brute Force`

## Evidence
- [x] Captured terminal alert evidence
- [x] Captured Wazuh event-detail evidence



## Result
- [x] **PASS — Custom high-value SSH detection successfully demonstrated**
