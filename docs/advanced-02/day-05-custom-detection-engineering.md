# Advanced 2 — Day 5: Custom Detection Engineering

## Objective

Create custom Wazuh detection rules for Windows security activity, validate the rules using controlled endpoint behaviour, and map detections to MITRE ATT&CK techniques.

## Environment

| System | Role | Internal IP |
| --- | --- | --- |
| `soc-ubuntu` | Wazuh Manager | `192.168.56.20` |
| `soc-linux-02` | Linux endpoint / Agent `001` | `192.168.56.30` |
| `soc-windows-01` | Windows 11 endpoint / Agent `002` | `192.168.56.40` |

## Pre-Deployment Validation

Existing Wazuh custom rules were reviewed before creating new Windows detection rules.

The current custom rules file was backed up:

```text
/var/ossec/etc/rules/local_rules.xml.a2-day5-backup
```

Wazuh rule syntax was validated:

```bash
sudo /var/ossec/bin/wazuh-analysisd -t
```

Result:

```text
Exit code: 0
```

## Custom Rule Development

Two Windows detection rules were created in:

```text
/var/ossec/etc/rules/local_rules.xml
```

## Custom Detection 1 — Suspicious PowerShell Execution

Rule:

```text
Rule ID: 100600
Level: 10
```

Detection:

```text
PowerShell executed with ExecutionPolicy Bypass
```

MITRE ATT&CK:

```text
T1059.001 — Command and Scripting Interpreter: PowerShell
```

Validation result:

```text
PASS — Custom PowerShell detection generated a Wazuh alert.
```

---

## Custom Detection 2 — Account Discovery Using net.exe

Rule:

```text
Rule ID: 100601
Level: 10
```

Detection:

```text
Account discovery using net.exe
```

Test commands:

```powershell
net user

net localgroup
```

Detected process:

```text
Image:
C:\Windows\System32\net.exe
```

MITRE ATT&CK:

```text
T1087 — Account Discovery
```

Validation result:

```text
PASS — Custom account discovery detection generated a Wazuh alert.
```

---

## Central Validation

Validated event information:

```text
Agent:
soc-windows-01

Provider:
Microsoft-Windows-Sysmon

Channel:
Microsoft-Windows-Sysmon/Operational

Event ID:
1
```

Custom detections:

```text
Rule: 100600
Level: 10

Rule: 100601
Level: 10
```

## Detection Flow

```text
Windows Activity
        |
        v
Sysmon Event ID 1
        |
        v
Wazuh Agent 002
        |
        v
Custom Wazuh Rule
        |
        v
SOC Alert Investigation
```


## Result

**PASS — Custom Windows detection rules were created, validated, and successfully generated SOC alerts through Wazuh.**

## Advanced 2 Status

```text
✅ A2-Day 1 — Build + network Windows endpoint

✅ A2-Day 2 — Wazuh Windows agent + telemetry

✅ A2-Day 3 — Sysmon deployment + Wazuh integration

✅ A2-Day 4 — Windows detection validation

✅ A2-Day 5 — Custom detection engineering
```

## Next

**Advanced 3 — Threat simulation, incident investigation, and detection improvement.**
