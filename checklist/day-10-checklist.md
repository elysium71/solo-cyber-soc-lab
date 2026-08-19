# Day 10 Checklist

## Auditd
- [x] Confirmed persistent b64/b32 `execve` rules
- [x] Confirmed Auditd key `audit-wazuh-c`
- [x] Verified loaded rules with `auditctl -l`
- [x] Generated controlled `sudo id` activity
- [x] Confirmed `AUID` preserved the logged-in user
- [x] Confirmed privileged execution used `EUID=0`

## Wazuh integration
- [x] Confirmed Auditd events reach Wazuh
- [x] Confirmed generic Rule `80792`
- [x] Confirmed privileged `id` event in `alerts.log`

## Custom detection
- [x] Created Rule `100500`
- [x] Set Level `11`
- [x] Used parent Rule `80792`
- [x] Matched `audit.auid=1000`
- [x] Matched `audit.euid=0`
- [x] Added exact anchors to reduce false positives
- [x] Added MITRE `T1548`
- [x] Validated with `wazuh-analysisd -t`
- [x] Triggered Rule `100500` live

## Validation
- [x] Positive test: `sudo id` triggered Rule `100500`
- [x] Confirmed `comm="id"`
- [x] Confirmed `AUID="socadmin"`
- [x] Confirmed root UID/EUID
- [x] Negative test: normal `id` remained on Rule `80792`
- [x] Confirmed normal `id` used UID/EUID `1000`
- [x] Verified Rule `100500` in Threat Hunting
