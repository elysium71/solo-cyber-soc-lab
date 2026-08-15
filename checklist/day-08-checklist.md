# Day 8 Checklist

## Auditd
- [x] Installed `auditd` and `audispd-plugins`
- [x] Confirmed Auditd active
- [x] Confirmed `/var/log/audit/audit.log`
- [x] Added persistent b64/b32 `execve` rules
- [x] Used key `audit-wazuh-c`
- [x] Verified loaded rules with `auditctl -l`

## Wazuh integration
- [x] Added Auditd `localfile` collector
- [x] Validated logcollector configuration
- [x] Restarted Wazuh Manager
- [x] Confirmed Wazuh analyzes `audit.log`
- [x] Confirmed generic Rule `80792`

## Custom detection
- [x] Created Rule `100300`
- [x] Set Level `12`
- [x] Used parent Rule `80792`
- [x] Matched `/etc/shadow`
- [x] Added MITRE `T1003`
- [x] Validated with `wazuh-logtest`
- [x] Confirmed Credential Access / OS Credential Dumping mapping
- [x] Triggered Rule `100300` live
- [x] Verified Rule `100300` in Threat Hunting

