# Day 8 — `/etc/shadow` Credential File Access Detection

## Objective
Detect privileged access to Linux credential material using Auditd and a custom Wazuh rule.

## Detection flow
```text
sudo cat /etc/shadow
        ↓
Auditd execve telemetry
        ↓
/var/log/audit/audit.log
        ↓
Wazuh Rule 80792
        ↓
Custom Rule 100300 (Level 12)
        ↓
MITRE ATT&CK T1003 — OS Credential Dumping
```

## Controlled test
Repository-safe testing used:

```bash
sudo cat /etc/shadow > /dev/null
```

This triggered Auditd without publishing credential material.

Audit evidence included:

```text
AUID="socadmin"
EUID="root"
comm="cat"
a0="cat"
a1="/etc/shadow"
key="audit-wazuh-c"
```

## Generic Wazuh detection
Wazuh first generated:

```text
Rule: 80792 (level 3) -> 'Audit: Command: /usr/lib/cargo/bin/coreutils/cat.'
```

Threat Hunting showed fields including `data.audit.execve.a1=/etc/shadow`, `data.audit.key=audit-wazuh-c`, and `decoder.name=auditd`.

## Custom rule
```xml
<group name="local,audit,credential_access,">
  <rule id="100300" level="12">
    <if_sid>80792</if_sid>
    <match>/etc/shadow</match>
    <description>Credential file access detected: /etc/shadow.</description>
    <mitre><id>T1003</id></mitre>
  </rule>
</group>
```

`<match>/etc/shadow</match>` was used because the standalone logtest input did not decode `audit.execve.a1`, while the complete event text contained `/etc/shadow`.

## Validation
`wazuh-logtest` returned:

```text
id: '100300'
level: '12'
description: 'Credential file access detected: /etc/shadow.'
mitre.id: '['T1003']'
mitre.tactic: '['Credential Access']'
mitre.technique: '['OS Credential Dumping']'
**Alert to be generated.
```

## Live result
A fresh controlled test generated:

```text
Rule: 100300 (level 12) -> 'Credential file access detected: /etc/shadow.'
```

Threat Hunting query:

```text
rule.id:100300
```

returned the custom Level 12 alerts.



Do not publish screenshots containing actual `/etc/shadow` contents or password hashes.

## Result
**PASS — Auditd → Wazuh → custom high-severity credential-file detection successfully demonstrated.**
