# Mini SOC Lab: Attack Simulation and Detection

A hands-on home Security Operations Centre (SOC) lab for safely simulating, detecting, investigating, and documenting security events with Kali Linux, Ubuntu Server, Wazuh, and Suricata.

> All testing is performed only against virtual machines owned and controlled by the project author on an isolated lab network.

## Project Overview

This project demonstrates a practical blue-team workflow: establish a secure lab, generate controlled security activity, inspect host and network telemetry, identify detection gaps, create custom rules, validate alerts, and map the activity to MITRE ATT&CK.

The environment uses Kali Linux as the attack-simulation system and Ubuntu Server as both the monitored target and the Wazuh server. Suricata adds network-level visibility, while Wazuh collects and analyses the resulting host and IDS events.

## Objectives

- Build an isolated and repeatable cybersecurity lab.
- Collect Linux authentication, system, and network IDS logs.
- Simulate authorized reconnaissance and account-related activity.
- Detect and investigate suspicious behaviour in Wazuh.
- Identify detection gaps and develop custom rules.
- Map observed activity to MITRE ATT&CK.
- Preserve technical evidence and documentation in GitHub.

## Lab Architecture

| System | Role | Internal IP |
| --- | --- | --- |
| Kali Linux (`soc-kali`) | Attack simulation | `192.168.56.10` |
| Ubuntu Server (`soc-ubuntu`) | Monitored target, Wazuh, and Suricata | `192.168.56.20` |

Each VM has two network adapters:

- **NAT:** Internet access for updates and software installation.
- **SOC-LAB:** Isolated internal network for controlled security testing.

## Detection Pipeline

```text
Kali Linux → Controlled activity → Ubuntu Server
                                      │
                         ┌────────────┴────────────┐
                         │                         │
                   Host logs                 Suricata IDS
                         │                         │
                         └────────────┬────────────┘
                                      ↓
                                    Wazuh
                                      ↓
                          Alert investigation and
                           MITRE ATT&CK mapping
```

## Current Progress

| Day | Focus | Status | Key outcome |
| --- | --- | --- | --- |
| 1 | Lab and network setup | Complete | Built two VMs, configured `SOC-LAB`, and verified ping and SSH connectivity. |
| 2 | Wazuh installation and validation | Complete | Installed the Wazuh stack and detected failed SSH authentication, including Rules `5710` and `5503`. |
| 3 | Suricata and Nmap detection | Complete | Integrated Suricata with Wazuh and detected an Nmap scan using Suricata SID `1000001` and Wazuh Rule `86601`. |
| 4 | Account and privilege monitoring | Complete | Detected account creation and built Wazuh Rule `100100` for sudo-group modification. |
| 5 | File Integrity Monitoring | In progress | Configure and validate Wazuh FIM for controlled sensitive-file changes. |

## Detection Highlights

### Nmap reconnaissance

An authorized `nmap -sV` scan initially exposed a network-visibility gap. Suricata was installed and integrated with Wazuh, then a custom TCP SYN threshold rule was added.

- Suricata SID: `1000001`
- Wazuh Rule: `86601`
- MITRE ATT&CK: **T1046 — Network Service Discovery**
- Result: **Detected**

See [Nmap Port Scan Detection](attack-simulations/day-03-nmap-scan.md).

### Sudo-group modification

A controlled test account was added to the privileged `sudo` group. The default Wazuh event did not clearly identify the privilege change, so a custom rule was developed and tested.

- Wazuh Rule: `100100`
- Alert level: `10`
- MITRE ATT&CK: **T1098 — Account Manipulation**
- Result: **Detected**

See [User Added to Sudo Group Detection](detection-rules/sudo-group-modification.md) and the [Wazuh XML rule](detection-rules/wazuh/sudo-group-modification.xml).

## Repository Structure

```text
solo-cyber-soc-lab/
├── architecture/          # Architecture material
├── attack-simulations/    # Controlled attack procedures and findings
├── checklist/             # Daily implementation checklists
├── detection-rules/       # Suricata and Wazuh custom detections
├── incident-reports/      # Investigation reports
├── screenshots/           # Evidence organised by lab day
├── scripts/               # Supporting automation
├── setup-guides/          # VM, network, and Wazuh setup notes
└── README.md
```

## Tools and Technologies

- Kali Linux
- Ubuntu Server
- Wazuh SIEM/XDR
- Suricata IDS
- Nmap
- Linux authentication and system logs
- Git and GitHub
- MITRE ATT&CK

## Skills Demonstrated

- SOC lab design and network isolation
- SIEM deployment and alert investigation
- Linux log analysis
- Network intrusion detection
- Detection-gap analysis
- Custom Suricata and Wazuh rule development
- Rule testing and alert validation
- MITRE ATT&CK mapping
- Evidence collection and technical documentation

## Planned Work

- Complete Wazuh File Integrity Monitoring validation.
- Create structured incident reports for the simulated events.
- Add more controlled authentication and persistence scenarios.
- Improve scripts and repeatability as the lab develops.

## Ethical Use

This repository is intended solely for defensive cybersecurity education. Security testing must be limited to systems owned by the tester or systems for which explicit authorization has been granted.
