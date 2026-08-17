# Day 9 --- Cron Persistence Detection

## Objective

Detect cron-based persistence using Wazuh real-time File Integrity
Monitoring and a custom Wazuh rule.

## Detection flow

``` text
Create file in /etc/cron.d
        ↓
Wazuh real-time FIM
        ↓
Wazuh Rule 554
        ↓
Custom Rule 100400 (Level 12)
        ↓
MITRE ATT&CK T1053.003 — Cron
```

## Controlled test

Repository-safe testing used:

``` bash
echo '* * * * * root /usr/bin/date >> /tmp/soc-cron-test.log' | \
sudo tee /etc/cron.d/soc-lab-persistence
sudo chmod 644 /etc/cron.d/soc-lab-persistence
```

The cron job was harmless and only wrote the current date to
`/tmp/soc-cron-test.log`.

Wazuh FIM evidence included:

``` text
File '/etc/cron.d/soc-lab-persistence' added
Mode: realtime
User: root (0)
Group: root (0)
```

## Generic Wazuh detection

Wazuh first generated:

``` text
Rule: 554 (level 5) -> 'File added to the system.'
```

The event confirmed that `/etc/cron.d/soc-lab-persistence` was detected
through real-time File Integrity Monitoring.

## Custom rule

``` xml
<group name="local,syscheck,persistence,">
  <rule id="100400" level="12">
    <if_sid>554</if_sid>
    <field name="file">^/etc/cron\.d/</field>
    <description>Persistence detected: new cron job created in /etc/cron.d.</description>
    <mitre><id>T1053.003</id></mitre>
  </rule>
</group>
```

The rule uses Wazuh Rule `554` as the parent and matches newly added
files under `/etc/cron.d`.

## Validation

The Wazuh rule configuration was validated with:

``` bash
sudo /var/ossec/bin/wazuh-analysisd -t
```

The command returned without errors.

## Live result

A fresh controlled test generated:

``` text
Rule: 100400 (level 12) -> 'Persistence detected: new cron job created in /etc/cron.d.'
File '/etc/cron.d/soc-lab-persistence' added
Mode: realtime
```

Threat Hunting query:

``` text
rule.id:100400
```

returned the custom Level 12 alert.

Threat Hunting confirmed:

``` text
rule.id: 100400
rule.level: 12
rule.mitre.id: T1053.003
rule.mitre.technique: Cron
syscheck.event: added
syscheck.mode: realtime
syscheck.path: /etc/cron.d/soc-lab-persistence
```

## Cleanup

The controlled persistence simulation was removed:

``` bash
sudo rm -f /etc/cron.d/soc-lab-persistence
sudo rm -f /tmp/soc-cron-test.log
```

The defensive Wazuh FIM configuration and custom Rule `100400` were
retained.

## Result

**PASS --- Wazuh FIM → custom high-severity cron persistence detection
successfully demonstrated.**
