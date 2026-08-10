# Day 5 Checklist --- File Integrity Monitoring

## Configuration

-   [x] Reviewed Wazuh FIM configuration
-   [x] Confirmed FIM was enabled
-   [x] Identified scheduled scan frequency
-   [x] Created controlled test file
-   [x] Recorded SHA-256 baseline
-   [x] Added dedicated real-time FIM monitoring
-   [x] Enabled `report_changes`
-   [x] Validated Syscheck configuration
-   [x] Restarted Wazuh successfully
-   [x] Confirmed real-time monitoring was active

## Detection

-   [x] Modified `/etc/soc-lab-monitor.conf`
-   [x] Calculated modified SHA-256
-   [x] Generated Wazuh Rule 550
-   [x] Observed Level 7 alert
-   [x] Confirmed `Integrity checksum changed`
-   [x] Confirmed `Mode: realtime`
-   [x] Captured old/new hashes
-   [x] Captured content diff
-   [x] Verified event in Wazuh dashboard

## MITRE ATT&CK

-   [x] Identified `T1565.001`
-   [x] Tactic: Impact
-   [x] Technique: Stored Data Manipulation

## Cleanup & Evidence

-   [x] Restored baseline content
-   [x] Confirmed original SHA-256 returned
-   [x] Confirmed restoration was detected
-   [x] Captured dashboard evidence
-   [x] Prepared Day 5 documentation
-   [x] Prepared reusable FIM configuration snippet


