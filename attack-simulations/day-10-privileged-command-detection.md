# Day 10 — Privileged Command Execution Detection

## Objective
Detect commands executed with root privileges by a logged-in non-root user using Auditd and a custom Wazuh rule.

## Detection flow
```text
socadmin → sudo command
              ↓
Auditd execve telemetry
              ↓
/var/log/audit/audit.log
              ↓
Wazuh Rule 80792
              ↓
Custom Rule 100500 (Level 11)
              ↓
MITRE ATT&CK T1548 — Abuse Elevation Control Mechanism
```

## Controlled test
```bash
sudo id
```

Audit evidence included:

```text
AUID="socadmin"
UID="root"
EUID="root"
comm="id"
audit.auid: 1000
audit.uid: 0
audit.euid: 0
audit.key: audit-wazuh-c
```

## Generic Wazuh detection
Wazuh first generated:

```text
Rule: 80792 (level 3) -> 'Audit: Command: /usr/lib/cargo/bin/coreutils/id.'
```

## Custom rule
```xml
<group name="local,audit,privilege_escalation,">
  <rule id="100500" level="11">
    <if_sid>80792</if_sid>
    <field name="audit.auid">^1000$</field>
    <field name="audit.euid">^0$</field>
    <description>Privileged command execution: non-root user executed command as root.</description>
    <mitre><id>T1548</id></mitre>
  </rule>
</group>
```

Exact anchors were used because an unanchored `0` could also match `1000`, creating false positives.

## Positive validation
A fresh `sudo id` generated:

```text
Rule: 100500 (level 11) -> 'Privileged command execution: non-root user executed command as root.'
```

with `audit.auid=1000`, `audit.uid=0`, and `audit.euid=0`.

## Negative control
A normal:

```bash
id
```

generated generic Rule `80792` with:

```text
audit.auid: 1000
audit.uid: 1000
audit.euid: 1000
```

Rule `100500` did not fire for the unprivileged `id` event.

## Threat Hunting
Dashboard query:

```text
rule.id:100500
```

confirmed the custom privileged-command alerts.

## Result
**PASS — Auditd → Wazuh → custom privileged-command detection successfully demonstrated, including positive validation and a negative control.**
