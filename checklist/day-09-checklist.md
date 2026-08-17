# Day 9 Checklist

## Cron baseline
- [x] Confirmed cron service active
- [x] Reviewed `/etc/cron.d`
- [x] Confirmed no existing root crontab
- [x] Confirmed no existing SOC lab cron persistence file

## Auditd
- [x] Added persistent watch for `/etc/cron.d`
- [x] Used key `cron-persistence`
- [x] Loaded rule with `augenrules`
- [x] Verified rule with `auditctl -l`
- [x] Confirmed cron file creation was recorded by Auditd
- [x] Confirmed `auid=socadmin`
- [x] Confirmed file creation occurred with root privileges

## Wazuh integration
- [x] Added real-time FIM monitoring for `/etc/cron.d`
- [x] Enabled `report_changes`
- [x] Validated Syscheck configuration
- [x] Restarted Wazuh Manager
- [x] Confirmed `/etc/cron.d` real-time monitoring in `ossec.log`
- [x] Confirmed generic Rule `554`

## Custom detection
- [x] Created Rule `100400`
- [x] Set Level `12`
- [x] Used parent Rule `554`
- [x] Matched new files under `/etc/cron.d`
- [x] Added MITRE `T1053.003`
- [x] Validated with `wazuh-analysisd -t`
- [x] Triggered Rule `100400` live
- [x] Verified Rule `100400` in Threat Hunting
- [x] Confirmed Cron technique mapping
- [x] Confirmed Persistence tactic mapping

## Cleanup
- [x] Removed `/etc/cron.d/soc-lab-persistence`
- [x] Removed `/tmp/soc-cron-test.log`
- [x] Retained defensive Auditd monitoring
- [x] Retained Wazuh FIM monitoring
- [x] Retained custom Rule `100400`