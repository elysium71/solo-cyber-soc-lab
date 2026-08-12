# Day 6 — SSH Brute-Force Detection with Wazuh

## Objective
Demonstrate Wazuh detection and correlation of repeated failed SSH authentication attempts from Kali Linux.

**Detection path:** Kali attack simulation → Ubuntu SSH logging → Wazuh detection → correlation → MITRE ATT&CK mapping.

## Lab Environment

| System | Role | SOC IP |
|---|---|---|
| Kali Linux | Attack simulation host | `192.168.56.10` |
| Ubuntu / Wazuh | Monitored SOC host | `192.168.56.20` |

## 1. Initial SSH Test
A controlled login attempt was made from Kali:

```bash
ssh invaliduser@192.168.56.20
```

Ubuntu `/var/log/auth.log` recorded:
```text
Invalid user invaliduser from 192.168.56.10
pam_unix(sshd:auth): authentication failure
Failed password for invalid user invaliduser from 192.168.56.10
```

## 2. Wazuh Individual Detections
Wazuh generated:

- **5710 / Level 5** — `sshd: Attempt to login using a non-existent user`
- **5503 / Level 5** — `PAM: User login failed.`
- **2502 / Level 10** — `syslog: User missed the password more than one time`

## 3. Wazuh Brute-Force Correlation Rule
The built-in SSH rule was inspected:

```bash
sudo grep -A 8 'rule id="5712".*level="10"' /var/ossec/ruleset/rules/0095-sshd_rules.xml
```

Relevant configuration:
```xml
<rule id="5712" level="10" frequency="8" timeframe="120" ignore="60">
  <if_matched_sid>5710</if_matched_sid>
  <same_source_ip />
  <description>sshd: brute force trying to get access to the system. Non existent user.</description>
  <mitre>
    <id>T1110</id>
  </mitre>
</rule>
```

This requires eight matching Rule 5710 events within 120 seconds from the same source IP.

## 4. Controlled Simulation
From Kali:

```bash
for i in {1..10}; do
    echo "=== SSH test attempt $i ==="
    ssh -o BatchMode=yes         -o PreferredAuthentications=password         -o PubkeyAuthentication=no         invaliduser@192.168.56.20
    sleep 2
done
```

The test was performed only against the isolated lab host.

## 5. Successful Detection
Wazuh generated:

```text
Rule: 5712 (level 10)
sshd: brute force trying to get access to the system. Non existent user.
Src IP: 192.168.56.10
```

Eight correlated events occurred approximately every two seconds from `10:08:52` through `10:09:06`, matching the configured frequency threshold.

## 6. Dashboard Investigation
Threat Hunting query:

```text
rule.id:5712
```

Expanded event details confirmed:

| Field | Value |
|---|---|
| `agent.name` | `soc-ubuntu` |
| `data.srcip` | `192.168.56.10` |
| `data.srcuser` | `invaliduser` |
| `decoder.name` | `sshd` |
| `rule.id` | `5712` |
| `rule.level` | `10` |
| `rule.frequency` | `8` |
| `rule.mitre.id` | `T1110` |
| `rule.mitre.tactic` | `Credential Access` |
| `rule.mitre.technique` | `Brute Force` |

## MITRE ATT&CK
**T1110 — Brute Force**, under the **Credential Access** tactic.

## Detection Flow

```text
Kali 192.168.56.10
        ↓
Repeated invalid SSH attempts
        ↓
Ubuntu sshd logs
        ↓
Wazuh Rule 5710
        ↓
8 events / 120 seconds / same source IP
        ↓
Wazuh Rule 5712 — Level 10
        ↓
MITRE ATT&CK T1110 — Brute Force
```

## Evidence

```text
screenshots/Day6
```

## Result
**PASS**

Day 6 demonstrated end-to-end SSH brute-force detection. Wazuh received Ubuntu authentication telemetry, detected individual invalid-user events, correlated repeated attempts from the Kali source IP into Rule 5712 Level 10, and mapped the alert to MITRE ATT&CK T1110.
