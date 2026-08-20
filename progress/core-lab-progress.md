# Core Lab Progress

The core Mini SOC Lab establishes the baseline environment and demonstrates a complete defensive workflow using Kali Linux, Ubuntu Server, Wazuh, Suricata, Auditd, File Integrity Monitoring, custom detections, MITRE ATT&CK mapping, and event correlation.

## Progress

| Status | Day | Focus | Key outcome |
| --- | --- | --- | --- |
| ✅ | **Day 1** | Lab and network setup | Built Kali and Ubuntu VMs, configured the isolated `SOC-LAB` network, and verified connectivity. |
| ✅ | **Day 2** | Wazuh installation and validation | Installed Wazuh and validated failed SSH authentication telemetry with Rules `5710` and `5503`. |
| ✅ | **Day 3** | Suricata and Nmap detection | Integrated Suricata with Wazuh and detected Nmap reconnaissance using Suricata SID `1000001` and Wazuh Rule `86601`. |
| ✅ | **Day 4** | Account and privilege monitoring | Detected account activity and created Rule `100100` for sudo-group modification. |
| ✅ | **Day 5** | File Integrity Monitoring | Configured and validated Wazuh FIM for controlled file changes. |
| ✅ | **Day 6** | SSH authentication detection | Investigated repeated SSH authentication activity and validated Wazuh authentication telemetry. |
| ✅ | **Day 7** | Custom SSH detection | Created Rule `100200` for high-value SSH attempts against the root account and mapped it to MITRE ATT&CK `T1110`. |
| ✅ | **Day 8** | Auditd credential-access detection | Integrated Auditd and created Rule `100300` for `/etc/shadow` access, mapped to `T1003`. |
| ✅ | **Day 9** | Cron persistence detection | Added real-time monitoring of `/etc/cron.d` and Rule `100400`, mapped to `T1053.003`. |
| ✅ | **Day 10** | Privileged command execution | Used Auditd telemetry and Rule `100500` to detect commands executed as root by a non-root login identity. |
| ✅ | **Day 11** | Network + host correlation | Correlated a Suricata port-scan alert (`86601`) with subsequent SSH invalid-user activity (`5710`) from the same source IP. |
| ✅ | **Day 12** | Final SOC lab review | Performed the final core-lab review, cleaned repository documentation, and prepared the project for the advanced phase. |

## Completion

**12 / 12 core lab days complete.**

## Core Skills Demonstrated

- Isolated SOC lab design
- Wazuh SIEM/XDR deployment
- Suricata IDS integration
- Linux authentication monitoring
- Auditd telemetry collection
- File Integrity Monitoring
- Custom Wazuh rule development
- Custom Suricata detection
- Detection validation
- MITRE ATT&CK mapping
- Network and host event correlation
- Evidence collection and Git documentation

## Next Phase

The project now continues into the [Advanced SOC Roadmap](advanced-roadmap.md), beginning with multi-endpoint monitoring.
