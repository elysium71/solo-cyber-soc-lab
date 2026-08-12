# Day 6 Checklist — SSH Brute-Force Detection

## Objective

Validate Wazuh's ability to detect individual failed SSH authentication attempts and correlate repeated attempts into a higher-severity brute-force alert.

## Environment Verification

- [x] Confirmed Kali SOC interface: `192.168.56.10`
- [x] Confirmed Ubuntu SOC interface: `192.168.56.20`
- [x] Confirmed Kali can reach Ubuntu over the SOC network
- [x] Confirmed SSH service is active on Ubuntu
- [x] Confirmed Wazuh Manager is active

## SSH Authentication Testing

- [x] Generated controlled invalid-user SSH authentication attempts from Kali
- [x] Confirmed failed SSH events were written to Ubuntu authentication logs
- [x] Confirmed source IP `192.168.56.10` appeared in the events

## Wazuh Detection

- [x] Observed Rule `5710` — non-existent SSH user
- [x] Observed Rule `5503` — PAM login failure
- [x] Observed Rule `2502` — repeated password failures
- [x] Inspected built-in Rule `5712`
- [x] Confirmed Rule `5712` severity is Level `10`
- [x] Confirmed `frequency="8"`
- [x] Confirmed `timeframe="120"`
- [x] Confirmed `<same_source_ip />` correlation
- [x] Confirmed Rule `5712` depends on matching Rule `5710`

## Brute-Force Correlation Test

- [x] Generated repeated controlled SSH attempts from Kali
- [x] Successfully triggered Wazuh Rule `5712`
- [x] Confirmed correlated source IP: `192.168.56.10`
- [x] Confirmed Wazuh retained the preceding authentication events
- [x] Verified Rule `5712` in Wazuh Threat Hunting
- [x] Verified expanded event details in Wazuh

## MITRE ATT&CK

- [x] Confirmed MITRE ATT&CK ID `T1110`
- [x] Confirmed technique: **Brute Force**
- [x] Confirmed tactic: **Credential Access**

## Documentation and Evidence

- [x] Created `attack-simulations/day-06-ssh-brute-force.md`
- [x] Captured Wazuh SSH brute-force rule evidence
- [x] Captured terminal Rule `5712` alert evidence
- [x] Captured Wazuh Threat Hunting Rule `5712` evidence
- [x] Captured expanded Rule `5712` event details


## Day 6 Result

- [x] **PASS — SSH brute-force detection and correlation successfully demonstrated**
- [x] Individual authentication failures were detected
- [x] Repeated events were correlated into a Level 10 alert
- [x] Detection was mapped to MITRE ATT&CK `T1110 — Brute Force`


