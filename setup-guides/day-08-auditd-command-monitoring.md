# Day 8 — Auditd Command Monitoring Setup

## Objective
Add persistent Linux command-execution telemetry to the Mini SOC by integrating Auditd with Wazuh.

## Installation and verification
Auditd was installed with:

```bash
sudo apt update
sudo apt install auditd audispd-plugins -y
```

Verification showed the service active, `auditctl version 4.1.2`, and `/var/log/audit/audit.log` present.

## Wazuh Auditd ingestion
The following collector was added to `/var/ossec/etc/ossec.conf` before `</ossec_config>`:

```xml
<localfile>
  <log_format>audit</log_format>
  <location>/var/log/audit/audit.log</location>
</localfile>
```

Validation and restart:

```bash
sudo /var/ossec/bin/wazuh-logcollector -t
sudo systemctl restart wazuh-manager
sudo systemctl is-active wazuh-manager
```

Wazuh confirmed:

```text
wazuh-logcollector: INFO: (1950): Analyzing file: '/var/log/audit/audit.log'.
```

## Persistent execve rules
`/etc/audit/rules.d/audit.rules` contains:

```text
-a always,exit -F arch=b64 -S execve -k audit-wazuh-c
-a always,exit -F arch=b32 -S execve -k audit-wazuh-c
```

They were loaded with:

```bash
sudo augenrules --load
sudo auditctl -l
```

## Verification
`ausearch` confirmed `EXECVE`/`SYSCALL` events with `key=audit-wazuh-c`. Wazuh ingested the telemetry and generated generic Audit command Rule `80792` alerts.

## Result
**PASS — Auditd command execution monitoring is persistent and integrated with Wazuh.**
