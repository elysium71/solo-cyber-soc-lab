# Day 5 --- File Integrity Monitoring with Wazuh

## Objective

Configure and validate Wazuh File Integrity Monitoring (FIM) against a
controlled configuration file. The workflow establishes a baseline,
modifies the file, detects the change in real time, investigates the
alert, maps it to MITRE ATT&CK, and restores the baseline.

## Environment

-   Wazuh Manager: `soc-ubuntu`
-   Monitored file: `/etc/soc-lab-monitor.conf`
-   Detection: Wazuh Syscheck / FIM
-   Mode: Real time
-   Wazuh Rule: `550`
-   Alert Level: `7`

## Baseline

The controlled test file was created with:

``` bash
echo "SOC Lab FIM Baseline" | sudo tee /etc/soc-lab-monitor.conf
```

Original SHA-256:

``` text
841fea6f4c5e1566dd377746fcd4c7baad6558fd08a48eac265ccf5f3fd136c1
```

The existing `/etc` monitoring was scheduled, with a scan frequency of
43200 seconds (12 hours). For an interactive detection test, a dedicated
real-time entry was added.

## Real-Time FIM Configuration

Inside the existing `<syscheck>` section of `/var/ossec/etc/ossec.conf`:

``` xml
<directories realtime="yes" report_changes="yes">/etc/soc-lab-monitor.conf</directories>
```

Configuration validation:

``` bash
sudo /var/ossec/bin/wazuh-syscheckd -t
```

After restarting Wazuh, `ossec.log` confirmed the path was monitored
with `report_changes` and `realtime`.

## Detection Test

The monitored file was modified:

``` bash
echo "Second unauthorized change - realtime FIM test" | \
sudo tee -a /etc/soc-lab-monitor.conf
```

Modified SHA-256:

``` text
60e1d92a7a71f25cb1feb804c01fcbc856625c0feea0a5d6d1db293c2c2c46e0
```

Wazuh generated:

``` text
Rule: 550
Level: 7
Description: Integrity checksum changed.
File: /etc/soc-lab-monitor.conf
Mode: realtime
Event: modified
```

Changed attributes included size, modification time, MD5, SHA-1, and
SHA-256.

Because `report_changes="yes"` was enabled, Wazuh captured the content
difference:

``` text
2a3
> Second unauthorized change - realtime FIM test
```

## MITRE ATT&CK

The alert was mapped to:

-   `T1565.001`
-   Tactic: Impact
-   Technique: Stored Data Manipulation

## Dashboard Verification

The event was verified in the Wazuh dashboard. The alert showed Rule
550, Level 7, real-time monitoring, the changed attributes, content
diff, and MITRE ATT&CK mapping.

## Evidence

```text
screenshots/Day5
```

## Cleanup

The file was restored:

``` bash
echo "SOC Lab FIM Baseline" | sudo tee /etc/soc-lab-monitor.conf
```

Restored SHA-256:

``` text
841fea6f4c5e1566dd377746fcd4c7baad6558fd08a48eac265ccf5f3fd136c1
```

The restored hash exactly matched the original baseline. Wazuh also
generated another Rule 550 alert for the restoration.

## Result

``` text
Baseline
  ↓
Controlled modification
  ↓
Wazuh real-time FIM
  ↓
Rule 550 / Level 7
  ↓
Hash and content change analysis
  ↓
MITRE T1565.001
  ↓
Dashboard investigation
  ↓
Baseline restoration
```

## Key Takeaways

-   Cryptographic hashes provide evidence of file modification.
-   Scheduled FIM can introduce detection delay.
-   Real-time FIM provides immediate visibility.
-   `report_changes` provides content-level investigation context.
-   Wazuh enriches FIM detections with MITRE ATT&CK information.
-   Controlled cleanup verifies the monitoring pipeline remains
    operational.
