# Day 7 — Custom High-Value SSH Account Detection

## Objective
Create, validate, and deploy a custom Wazuh detection rule that raises a high-severity alert when a failed SSH authentication attempt targets the `root` account.

Day 7 extends the Day 6 SSH work from built-in Wazuh correlation into custom detection engineering.

## Lab Environment

| System | Role | SOC IP |
|---|---|---|
| Kali Linux | Attack simulation host | `192.168.56.10` |
| Ubuntu / Wazuh | Monitored SOC host | `192.168.56.20` |

## Detection Flow

```text
Kali 192.168.56.10
        ↓
Failed SSH login targeting root
        ↓
Ubuntu sshd / journald
        ↓
Wazuh SSH decoder and parent rule
        ↓
Custom Rule 100200
        ↓
Level 12 alert
        ↓
MITRE ATT&CK T1110 — Brute Force
```

## 1. Backup Existing Rules

```bash
sudo cp /var/ossec/etc/rules/local_rules.xml \
/var/ossec/etc/rules/local_rules.xml.day7-backup
```

The existing custom Rule `100100` was preserved.

## 2. Final Custom Rule

```xml
<group name="local,syslog,sshd,">

  <rule id="100200" level="12">
    <if_sid>5716</if_sid>
    <user>root</user>
    <description>High-value SSH login attempt using root account.</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>

</group>
```

Rule properties:

- Rule ID: `100200`
- Severity: Level `12`
- Parent rule: `5716`
- Target user: `root`
- MITRE ATT&CK: `T1110 — Brute Force`
- Tactic: `Credential Access`

## 3. Rule Validation

```bash
sudo /var/ossec/bin/wazuh-logtest
```

Test event:

```text
Aug 14 19:31:33 soc-ubuntu sshd[121673]: Failed password for root from 192.168.56.10 port 52282 ssh2
```

Successful Phase 3 result:

```text
id: '100200'
level: '12'
description: 'High-value SSH login attempt using root account.'
mitre.id: '['T1110']'
mitre.tactic: '['Credential Access']'
mitre.technique: '['Brute Force']'
**Alert to be generated.
```

## 4. Live Test

The corrected rule was loaded by restarting Wazuh:

```bash
sudo systemctl restart wazuh-manager
sudo systemctl is-active wazuh-manager
```

A controlled failed SSH authentication attempt was then generated from Kali:

```bash
ssh root@192.168.56.20
```

An intentionally incorrect password was used.

## 5. Live Wazuh Alert

The alert was checked with:

```bash
sudo grep -B 5 -A 15 "Rule: 100200" \
/var/ossec/logs/alerts/alerts.log | tail -40
```

Wazuh generated:

```text
Rule: 100200 (level 12) -> 'High-value SSH login attempt using root account.'
Src IP: 192.168.56.10
User: root
Failed password for root from 192.168.56.10
```

This demonstrated that Rule `100200` worked in the live detection pipeline.

## 6. Threat Hunting Verification

Query:

```text
rule.id:100200
```

Expanded event details confirmed:

| Field | Value |
|---|---|
| `agent.name` | `soc-ubuntu` |
| `data.dstuser` | `root` |
| `data.srcip` | `192.168.56.10` |
| `decoder.name` | `sshd` |
| `rule.id` | `100200` |
| `rule.level` | `12` |
| `rule.mitre.id` | `T1110` |
| `rule.mitre.tactic` | `Credential Access` |
| `rule.mitre.technique` | `Brute Force` |

## 7. Troubleshooting During Development

Three issues were identified and corrected during development:

1. The new SSH `<group>` was initially nested inside the existing account-change group. The XML structure was corrected.
2. Rule `5710` was initially used as the parent, but it applies to non-existent-user SSH events and did not match a real `root` authentication failure.
3. `<field name="srcuser">root</field>` was rejected because the username is a static decoder field. The final rule uses `<user>root</user>`.

These corrections were validated before the final live test.

## 8. Evidence

```text
screenshots/Day7/day7-custom-root-ssh-alert-terminal.png
screenshots/Day7/day7-custom-root-ssh-event-details.png
```

## 9. Detection Rule Export

```text
detection-rules/day-07-high-value-ssh-root.xml
```

## Result

**PASS**

Day 7 demonstrated custom Wazuh detection engineering. Rule `100200` was created, debugged, validated with `wazuh-logtest`, deployed to the live manager, triggered by a controlled Kali SSH attempt, and verified in Wazuh Threat Hunting with MITRE ATT&CK `T1110 — Brute Force`.
