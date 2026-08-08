# Day 3 Checklist — Suricata IDS & Nmap Detection

[x] Verified Wazuh Manager is active
[x] Verified Wazuh Indexer is active
[x] Verified Wazuh Dashboard is active

[x] Verified Kali → Ubuntu network connectivity
[x] Confirmed Kali IP: 192.168.56.10
[x] Confirmed Ubuntu SOC IP: 192.168.56.20

[x] Installed Suricata IDS
[x] Updated Suricata rules with suricata-update
[x] Configured Suricata to monitor ens34
[x] Verified Suricata configuration
[x] Verified Suricata service is active

[x] Confirmed Suricata writes eve.json
[x] Configured Wazuh to ingest Suricata eve.json
[x] Restarted and verified Wazuh Manager

[x] Ran controlled Nmap scan from Kali
[x] Identified initial network detection gap
[x] Created custom Suricata port-scan rule
[x] Added local.rules to Suricata
[x] Verified custom rule loads successfully

[x] Triggered custom Suricata alert
[x] Confirmed SID 1000001 in Suricata
[x] Confirmed Suricata alert appears in Wazuh
[x] Confirmed Wazuh Rule ID 86601
[x] Mapped activity to MITRE ATT&CK T1046
[x] Documented Nmap detection test